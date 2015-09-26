

Ich sitze zur Abwechselung mal wieder an einem recht interessanten Design-Problem zu dem ich gerne eure Meinungen und Ratschläge abfragen würde. 
Ich versuche mal die Problemstellung, Lösungsidee und meine Herausforderungen möglichst knapp zu beschreiben:

# Problemstellung 
Ein REST Service liefert bei einer Anfrage u.U. eine große Menge an Datensätzen an eine aufrufende Client Applikation. 
Der REST Service ist in Java Implementiert (mit Spring und Jersey 2). Die Daten werden über JDBC aus der Datenbank gelesen. Die Rows aus dem JDBC ResultSet müssen dabei noch zu Ergebnisobjekten aggregiert werden (flaches ResultSet einer hierarchischen Ergebnisstruktur).
Die Datenbank Tabellen sind mit VPD Policies versehen weswegen vor und nach der Datenbank Abfrage ein PL/SQL Call abgesetzt werden muss um den Security Context zu initialisieren und abschließend wieder zurückzusetzen.
Die Ergebnisdatensätze sollen gestreamed geliefert werden, damit die Client Applikation schon die ersten Datensätze verarbeiten kann während der REST Service noch die restlichen ausliefert.

# Design Idee
Um die Response zu streamen sieht die JAX-RS Resource die Verwendung eines javax.ws.rs.core.StreamingOutput vor.
Da ich das JAX-RS StreamingOutput nicht an die Persistenzschicht weitergeben möchte nutze ich eine java.util.concurrent.BlockingQueue als Transport-Medium.
Damit ich die Response schon starten kann während die JDBC Verarbeitung noch läuft starte ich die JDBC Abfrage in einem separaten Thread. Dazu erstelle ich einen Query Processor welcher die DataSource und die Query bekommt und die komplette Abfrage (Initialisieren des Security Context, ausführen der Query, verarbeiten des ResultSet, erzeugen der Ergebnis Objekte) ausführt und die Ergebnis Objekte in die Queue schreibt.
Zum starten und verwalten der nebenläufigen Prozesse nutze ich den java.util.concurrent.ExecutorService (als Spring Bean).
Die StreamingOutput Implementierung liest die Ergebnis Objekte aus der Queue und schreibt sie als JSON String in die Response. Durch die Verwendung der BlockingQueue wird dabei ggf. gewartet bis der Query Processor die nächsten Datensätze bereitstellt. Ein spezielles End-Element (Poison-Element) kennzeichnet das Ende des Ergebnisses.

Im Großen und Ganzen ist unsere Lösung so geblieben wie zuvor geschildert:
1. Die REST QueryResource nimmt den Request entgegen, prüft die Parameter und ruft einen QueryService zum Ausführen der Query auf
2. Der QueryService erzeugt über eine QueryWorkerFactory eine neue QueryWorker (Runnable) Instanz zur Ausführung der Query.
3. Die QueryWorkerFactory initialisiert den QueryWorker mit den notwendigen Resourcen zur Ausführung der Datenbank Anfrage (i.W. DataSource und TransactionManager) sowie das auszuführende Query Objekt. 
4. Der QueryWorker initialisiert eine BlockingQueue zur asynchronen Bereitstellung der Ergebnis Datensätze.
5. Der QueryService übergibt die QueryWorker Instanz an einen Spring SimpleAsyncTaskExecutor zur ansynchronen Ausführung und liefert die BlockingQueue aus dem QueryWorker an die aufrufende QueryResource zurück.
6. Die QueryResource nimmt die BlockingQueue des Services entgegen und wartet bis darin das erste Result Element verfügbar ist.
7. Der asynchron ausgeführte QueryWorker führt die Query unter Verwendung eines Spring JDBC Templates aus. 
7a. In der DB wird VPD eingesetzt um die Row-Level-Security sicherzustellen. Aus diesem Grund muss vor Ausführung der DB Anfrage (innerhalb einer Transaktion) ein Security Context gesetzt (und am Ende wieder zurückgesetzt) werden. 
7b. Da der QueryWorker in einem separaten Thread läuft kann keine Transactional Annotation verwendet werden. Stattdessen wird über ein Spring TransactionTemplate die Ausführung innerhalb einer Transaktion sichergestellt.
7c. Ein RowCallbackHandler wandelt die ResultSet Zeilen in Ergebnis Datensätze um und schreibt diese in die BlockingQueue.
8. Die QueryResource am anderen Ende der BlockingQueue empfängt den ersten Ergebnis Datensatz (mit einem peek(), Element bleibt in der Queue) und prüft diesen.
8a. Wenn es sich um das End Element handelt hat die Query keine Ergebnisse ergeben. Die QueryResource gibt in diesem Fall eine 404 Not Found Response zurück.
8b. Im anderen Fall (echtes Ergebnis Element) hat die Query Ergebnisse ergeben und die QueryResource gibt eine StreamingOutput Implementierung auf Basis der BlockingQueue zurück.
9. Das StreamingOutput liest alle Elemente (bis zum End Element) aus der BlockingQueue und schreibt diese (als JSON String) in die Response.
