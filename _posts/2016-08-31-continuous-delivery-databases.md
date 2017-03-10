---
title:  "Continuous Delivery von Datenbankänderungen"
categories:
  - "continuous delivery"
author: richard.attermeyer
---
Da rief mich doch gerade ein Kollege an und fragte, wie das denn geht mit Continuous Delivery und Datenbankänderungen.

Also ich kann nur berichten, wie wir das im aktuellen Projekt machen.

Generell benötigt man dazu ein Schemamigrationstool, denn auch die Schemamigration muss man automatisieren.
Dazu eignen sich Werkzeuge, wie

* [Liquibase](http://www.liquibase.org/)
* [Flyway](https://flywaydb.org/)
* oder wenn man Oracle Datenbanken verwaltet [Orcas](https://github.com/opitzconsulting/orcas)

Natürlich kann man mit Liquibase und Flyway auch Oracle Datenbanken verwalten. Orcas kann dann aber eine bessere Wahl sein.
Insbesondere, wenn man die [unterschiedliche Philosophie](http://opitzconsulting.github.io/orcas/docs/de/liquibase/) von Orcas gegenüber Flyway und Liquibase vorzieht.
Aber im Folgenden abstrahieren wir vom konkreten Produkt und zeigen wie man die Schemamigration in eine Continuous Delivery Pipeline
einbauen kann.

## Anforderungen an die Integration

Hier einige Anforderungen und Randbedingungen, die für uns wichtig waren, als wir das Schemamanagement integriert haben.

* Es werden die gleichen Mechanismen für alle Umgebungen (Entwicklung, Test, User Acceptance Test, Capacity Test, Pre-Production, Production) verwendet, um möglichst häufig die Migration zu testen, Inkonsistenzen durch unterschiedliches Vorgehen und Mehraufwand durch doppelte Pflege zu vermeiden.
* Die Migration wird als einmaliger Schritt im Rahmen des Deployments einer Umgebung betrachtet.
* Es muss weitestgehend möglich sein, die Migration von persistenten Umgebungen (Datenbank bleibt beim Neudeployment der Umgebung bestehen) vorher zu testen. Dies betrifft also insbesondere PreProduction und Production.
* Die Migration muss automatisiert durch Skripte erfolgen, damit sich durch manuelle Eingriffe keine Fehler einschleichen und der Aufwand gering bleibt.
* Wir müssen auch "Basisdaten" verwalten können. Dies umfasst z.B. Referenztabellen (z.B. Ländercodes).

## Kompromisse

Häufig müssen bei Qualitätskritieren Kompromisse eingeangen werden. Ein für uns sehr wichtiger Kompromiss ist der zwischen Qualität der Datenmigration und Dauer für bestimmte Schritte in der Delivery Pipeline.

Da wir für alle Umgebungen ein gleichartiges Vorgehen nutzen, bedeutet dies für einige Umgebungen, insbesondere bei denen es auch eine flüchtige Datenbank gibt (also ein DB, welche bei jedem neuen Deployment ihre Daten verliert), dass das Erzeugen des Schemas länger dauert als wenn man die Datenbank durch ein einziges Skript auf den aktuell gültigen Zustand bringt und nicht inkrementelle Änderungen anwendet (die ja mit der Zeit immer länger werden).
Dies hat Auswirkungen z.B. auf die Dauer von E2E Tests. Um dies zu kompensieren, kann die Nutzung von mehr Ressourcen notwendig sein. Die Zusatzkosten sind aber unserer Erfahrung nach geringer als die Kosten aufgrund doppelter Pflege und
Nacharbeiten als Folge von schlechter Qualität.  

## Lösungsmöglichkeit

Wir verwenden eine komplett auf Docker basierende Continuous Delivery Pipeline. Dies bedeutet, dass wir ab einem bestimmten Schritt nicht
mehr mit den jar / war Artefakten arbeiten, sondern direkt mit den erzeugten Docker Images.

Unter diesen Randbedingungen sieht unsere Lösung in etwa wie folgt aus:

* Wir verwenden Liquibase, da das Tool datenbankunabhängig ist und wir damit einfach MySQL, unterschiedliche Kontexte und Basisdaten verwalten können.
* Jede Applikation / Service enthält ein Modul "dbmigrations", welches alle notwendigen Liquibase Migrationen enthält (als SQL Dateien).
* In einem bestimmten Schritt erzeugen wir die entsprechenden Docker Images, unter anderem dabei auch das Liquibase Image. Dazu holen wir die dbmigrations Module aus dem Maven Repository. Dieses Image beinhaltet ein Skript, welches die Datenbankmigrationen dann auch auf eine Zieldatenbank anwenden kann.
* Es gibt einen weiteren Pipelineschritt, der frühzeitig feststellen soll, ob die Migrations fehlerfrei sind und auf eine bestehende Datenbank angewendet werden können. Dazu holen wir das Liquibase DB Migrations Image, welches aktuell auf der UAT Umgebung deployed wurde. Dieses wenden wir auf eine "leere" Datenbank an. Anschließend wird das aktuell gebaute Liqubiase Image auf dieser Datenbank angewendet. Dies setzt voraus, dass niemand manuell Änderungen an der Datenbank durchführt. Ansonsten sollte man immer das DDL direkt aus der Zieldatenbank (UAT, Prod) erzeugen, gegen welche man testen will.
* Das Deployment erfolgt in zwei Schritten. Die Umgebungen sind als docker-compose Dateien beschrieben. Mittels Docker Compose kann man das Deployment von mehreren zusammenhängenden Containern beschreiben, etwa auch, welche Container vor anderen gestartet sein müssen. Docker Compose reicht aber alleine nicht aus, um ein erfolgreiches Deployment sicher zu stellen. Dies liegt daran, dass Docker Compose abhängige Container direkt startet, wenn die Container, die als Abhängigkeit definiert sind, gestartet sind. Gestartet heißt aber nicht, dass der darin enthaltene Prozess wirklich "fertig" ist. Wir lösen dies, indem wir das docker-compose File mittels eines Shell Skripts nur bis zum liquibase Container starten und warten, dass sich dieser wieder (erfolgreich) beendet. Dann wissen wir, dass die Datenbank im neuen Zustand vorhanden ist und die Applikation starten kann.

### Diskussion
Einige Leser werden sich sicher fragen, warum wir für die Schemamigration einen eigenen Container verwenden und dies nicht direkt von der Anwendung ausführen lassen.
Auf diese Weise kann die Datenbankmigration unabhängig von der Anwendung ausgeführt werden. Dies ist wichtig, um z.B. Skripte zur Datenbankbefüllung während der Entwicklungs- und Test-Phasen zu testen. Diese Tests laufen teilweise auf Datenbanken, auf denen keine Anwendung zugreift.
Es war also ein Kompromiss: Mehr Flexibilität, aber auch mehr Komplexität aufgrund eines weiteren Containers gegenüber Einfachheit und eingeschränktere Einsatzmöglichkeiten.
