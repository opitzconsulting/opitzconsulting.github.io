--- 
layout: page
header: no
title: "DevOps und ITIL: Waffenbrüder oder Feinde? (Teil 2)"
author: richard.attermeyer
teaser: "DevOps und ITIL scheinen auf einen ersten oberflächlichen Blick unterschiedliche Ziele zu verfolgen. Die Wahrnehmung von ITIL ist, dass es eine Methode ist, die häufig in größeren Organisationen zum Einsatz kommt in denen Entwicklung und Betrieb stark voneinander getrennt arbeiten und dies auch so gewollt ist. DevOps aber wird häufig mit dem Einreißen der Mauern zwischen diesen
Organisationseinheiten in Verbindung gebracht. In dieser Blogserie zeigen wir, wie beide zusammen funktionieren können"
categories:
- automatisierung
- agilitaet
---

## ITIL Prozesse im Detail

Im Folgenden wollen wir uns die von uns umgesetzten ITIL Prozesse
anschauen. Dabei stellen wir dar, was die Aufgabe des Prozesses ist und
wie er mit Praktiken, wie sie sich im Umfeld von Continuous Delivery und
DevOps entwickelt haben umgesetzt werden können.

![]({{ site.baseurl }}/assets/img/devops_und_itil/prozesse1.png)

### Wartungsübernahme - Knowledge Management

Die Wartungsübernahme ist die Phase, in der wir uns schrittweise das
notwendige Wissen aneignen, um später die Wartung eigenverantwortlich zu
übernehmen.

Der entscheidende Prozess für die Wartungsübernahme ist das _Knowledge
Management_. ITIL Knowledge Management ist für das Erfassen, Analysieren,
Speichern und Bereitstellen von Wissen und Informationen innerhalb der
Organisation zuständig. Knowledge Management hat das primäre Ziel, Wissen
effizient verfügbar zu machen, so dass es nicht mehr notwendig ist, einmal
erworbenes Wissen aufwendig wiederzuerlangen.


Um die Informationen und Daten in geeigneter Form darzustellen, werden die
Applikationen mittels standardisierten Dokumentationen beschrieben. Diese
Informationen werden in einem zentralen System abgelegt. Wir planen durch
die Einführung eines zentralen IT Service Management-Tools für Applikation
und Infrastruktur die Dokumentation zu verbessern und zu zentralisieren.
Gerade bei Systemen die von OC|MSA(R) und OC|MSI(R) betreut werden,
erwarten wir uns Synergieeffekte.

Aus der agilen Entwicklung ist schon lange bekannt, dass man mit
schriftlicher Dokumentation nicht alle Aspekte abdecken kann. Genau so
wichtig ist die direkte Kommunikation zwischen den Beteiligten. Aus diesem
Grund sollten Wartungsmitarbeiter schon in der initialen Erstellungsphase
beteiligt sein. Insbesondere kann dann derjenige, der später die Wartung
durchführt auch die Dokumentation erstellen. Ansonsten spekulieren die
Entwickler nur, was für die Wartung später wichtig ist. 

Da während der Wartung die Software regelmäßig weiter entwickelt wird,
sollte die Dokumentation einfach änderbar abgelegt sein und den
Entwicklern entgegen kommen. Wir haben dabei gute Erfahrung mit der Ablage
von Wartungsdokumentation im Wiki gemacht. 

Prinzipien die uns in dieser Phase und beim Knowledge Management allgemein
leiten sind

* Ermögliche Kooperation zwischen den Stakeholdern
* Erleichtere Zugang zu Dokumentation
* Ermutige zu Änderungen von Dokumentation

Die Idee ist es die Hürden möglichst klein zu halten. Dokumentation, die
losgelöst vom Quellcode verwaltet wird tendiert generell dazu schnell zu
veralten. Die Gefahr ist um so größer, je aufwändiger jemand die Pflege
der Dokumentation empfindet.

#### Probleme beim Knowledge Management Leider werden durch
organisatorische Maßnahmen häufig eine Kooperation erschwert. Mit dem
Nachteil, dass Dokumentation mehrfach angelget und weiterentwickelt wird.
Der Wunsch ist eigentlich, dass man ein Product Team hat, welches für eine
Applikation verantwortlich ist. Amazon bezeichnet dies auch als
2-Pizza-Team. Dieses Team ist für alle Aspekte von der Entwicklung über
Wartung und den Betrieb der Anwendung verantwortlich. Leider sind viele
IT-Organisationen funktional aufgestellt und trennen zwischen
Anwendungsenticklung (und -wartung) und Basisbetrieb. Im Fall eines
externen Dienstleisters sieht dies dann häufig auch noch so aus, dass drei
Parteien miteinander sprechen:

* IT-Abteilung
* Outsourcing Partner für die Applikation
* Outsourcing Partner für den Basisbetrieb / Hosting

Gerade im Licht aktueller Entwicklungen wie Software-as-a-Service gibt es
einige Ideen, dieser Entwicklung gegenüber zu treten. Im Zuge von SaaS
kaufen Firmen die Weiterentwicklung und den Betrieb bei einem
Dienstleister ein. Warum übertragen sie dies nicht auch auf ihre eigene
IT-Abteilungen. Internetkonzerne wie Amazon, Netflix oder Facebook machen
es vor. Für eine Anwendung ist ein Produktteam zuständig, entweder als
querschnittlich besetztes Team (Entwickler, Analysten, Administratoren) in
der Firma oder als Managed Service Team bei einem Outsourcingpartner?

Wenn wir dies aber nicht kurzfristig ändern können, müssen wir mit der
funktionalen Trennung leben. Im Outsourcinggeschäft werden dann aber mit
den unterschiedlichen Partnern inkompatible Abrechnungssysteme vereinbart,
die unterschiedliche Anreize setzen. Der klassiche Gegensatz zwischen
Entwicklung und Betrieb, Anzahl neuer Features gegen Stabilität des
Systems, Kontinuierliche Änderungen gegen "never touch a running system".
Hier ist sicherlich Kreativität gefragt, ob die Abrechnungssysteme nicht
hinsichtlich der Maximierung des Geschäftswerts der Anwendung geändert
werden können. In diesem Fall würden  die Anreize der Beteiligten in die
gleiche Richtung ziehen.

Aus unserer Sicht kann aber auch die Orientierung von Unternehmen hin zu
Cloud basierten Infrastrukturen die Aufgaben der Partner verändern. Mit
der Enterprise Cloud hält die per Software definierte Infrastruktur einzug
in immer mehr Unternehmen. In diesem Fall ist der Hosting Provider für den
Betrieb der Cloud und die notwendigen Infrastrukturservices zuständig, der
Applikationsprovider hat Hoheit über die Code, die Libraries, den Package
Manager und ggf. auch das Betriebssystem. Mit Applikationscontainern wie
Docker wird die Linie immer unklarer, was Infrastruktur und was
Applikation ist.

### Release & Deployment Management

Ziel des Release und Deployment Managements ist Sicherstellung eines
zielgerichteten und sicheren Rollout der im Rahmen der Change Managements
geplanten Änderungen.  Dazu werden alle im Change Prozess genehmigten
Änderungen, die zu definierten Terminen entwickelt und getestet sein
sollen, zu einem Release zusammengefasst.

Eine wichtige Komponente dabei ist die Definitive Media Library, ein
virtuelles Konstrukt, dass alle relevanten Configuration Items
zusammenfasst. Continuous Delivery schlägt in die gleiche Kerbe und
fordert alles unter Versionskontrolle zu stellen:

* Quellcode
* Datenbanken-Schemata
* Infrastruktur

Große Änderungen ergeben sich gerade im Bereich Infrastruktur. In
klassischem Vorgehensweise war das entweder ein "Golden Image" oder ein
Installationsskript (häufig Word), das beschreibt, welche Komponenten wie
zu installieren sind. Änderungen zwischen Versionen sind dann nur schwer
nachvollziehbar. Dieses Vorgehen führte häufig dann aber auch dazu, dass
es kleine bis große Abweichungen zwischen unterschiedlichen Umgebungen
(Test, Produktion) gibt.

Infrastruktur wird bei Continuous Delivery Vorgehen als Code beschrieben
mittels Tools wie Ansible, Puppet oder Chef oder als Dockerfiles, wenn es
um Container geht. Dieser Code kann mit den gleichen Tools versioniert und
verwaltet werden, wie jeder andere Quellcode auch.

Erst wenn alles unter Versionskontrolle ist und mit festen Versionsnummern
versehen werden kann ist ein reproduzierbarer Build möglich.
Versionskontrolle ist somit eine Vorraussetzung aber längst nicht
hinreichend. 

Über das Konzept der Continuous Delivery Buildpipeline ist es dann möglich
aus den Versionsständen ein Release zu bauen. Die Continuous Delivery
Pipeline ist eine Weiterentwicklung von Continuous Integration. Dabei kann
eine Änderung (etwa ein Feature) durch  das System „fließen“. Auf dem Weg
wird diese Änderung verschiedene Stationen durchlaufen. Jede Station
versucht die Qualität des veränderten Systems zu untersuchen und
sicherzustellen, dass es zu keinen negativen Auswirkungen in Produktion
kommt. Die Pipeline soll dem Team möglichst frühzeitig Feedback geben und
zeigen, wo eine Änderung auf dem Weg in die Produktion steht. Je mehr
Stages eine Änderung durchlaufen hat, desto mehr Vertrauen gibt es in die
Produktionsreife. Gerade bei den Test und Abnahmeumgebungen soll der
Installationsprozess genau so (automatisch) vorgenommen werden, wie er
später in Produktion passiert.

Die Buildpipeline ist also das Mittel, um die wesentlichen Prinzipien
umzusetzen:

* Baue alle Binärartefakte genau einmal
* Deploye alle Umgebungen auf gleicher Weise
* Halte die Umgebungen gleich
* Wenn etwas fehlschlägt, halte die Pipeline an

Ein großes Problem war häufig die Umgebungen gleich zu halten. Ein
wesentlicher Schritt ist es die Umgebungen nicht mehr manuell zu ändern,
sondern dies über Konfigurationsmanagementwerkzeuge zu machen. Um die
Abhängigkeiten von der Umgebung weiter zu reduzieren, kann aber die
Deploymenteinheit vergrößert werden. Man könnte die ganze virtuelle
Maschine nehmen, aber das ist aufgrund der Größe (GB Bereich) häufig zu
schwerfällig. Mit Docker steht ein Vertreter von Applikationscontainern
bereit, der in den letzten 2 Jahren die Continuous Delivery Gemeinschaft
erobert hat. Verkürzt umrissen stellt ein Dockercontainer eine
leichtgewichtige virtuelle Maschine dar. Die Anwendung bringt innerhalb
des Dockercontainers alle Abhängigkeiten mit (etwa Java Version, Ruby
Gems, etc.) und garantiert, dass sie in der erwarteten Version vorliegt.
Ein solcher Container kann dann von Umgebung zu Umgebung geschoben werden.
Damit ist viel wahrcheinlicher, dass eine Applikation auf jeder Umgebung
läuft, auf der Docker Container ausgeführt werden können.

### Change Management

Über den ITIL Changemanagementprozess wird der Lebenszyklus aller Changes
gesteuert. Das Ziel des Changemanagements  ist die Sicherstellung, dass
Änderungen einer kontrollierten Weise registriert, bewertet, autorisiert,
priorisiert, geplant, geprüft, durchgeführt und dokumentiert wird. Durch
die genaue Bewertung und   Prüfung der Changes soll sichergestellt werden,
das nur nutzbringende Changes autorisiert werden und die negativen
Auswirkungen auf die Applikation vermieden werden. 

Häufig eine genaue Bewertung und Prüfung notwendig, da eine Release selten
erfolgt und damit in einem Release viele Änderung enthalten sind. Leider
lassen sich die Auswirkungen in der Gesamtheit häufig nicht komplett
bewerten. Im Rahmen von Continuous Delivery wird viel häufiger (einige
Firmen schaffen es mehrmals am Tag) ein Release auf Produktion ausgerollt.
Je kleiner die Änderung ist, desto geringer sind in der Regel die
möglichen negativen Auswirkungen. Insbesondere, wenn die Änderung auf
vorgelagerten Systemen schon im Zusammenspiel mit anderen Systemteilen
getestet wurde. Klassisch war die Notfallstrategie, wenn irgend etwas im
Release schief ging das ganze Release zurückzurollen. Wer aber hat das
Rückrollen vorher regelmäßig für jedes Release auf den
Pre-Produktionsumgebungen getestet. Nicht repräsentative Umfragen in
Vorträgen zu dem Thema lösten in der Regel heiteres Gelächter aus. Selbst
wenn eine Rücknahme des Releases erfolgreich ist, werden damit auch häufig
viele nutzbringende Änderungen nicht ausgerollt. Bei viel kleineren
Änderungen kann ein Fehler viel eindeutiger und schneller lokalisiert und
behoben werden. Mit einer guten Buildpipeline ist diese Änderung dann auch
relativ schnell und getestet in Produktion ausgerollt. DevOps hebt daher
die Bedeutung der Zeit zur Fehlerbehebung hervor, statt die Zeit zwischen
Fehlern zu verbessern. Eine wichtige Frage in diesem Zusammenhang lautet
daher: Wie lange dauert es in Ihrem Unternehmen die Änderung einer Zeile
Code in die Produktion auszurollen?

Da Releases nur selten gemacht werden, wird auch die Installation auf die
verschiedenen Umgebungen häufig nur selten gemacht und daher häufig
manuell durchgeführt. Typischerweise gibt es in vielen Firmen noch den
Iterativen Wasserfall.

![]({{ site.baseurl }}/assets/img/devops_und_itil/Iterativer_Wasserfall.png)

Aber getreu dem Motto: "Wenn etwas weht tut, mache es öfter", wird auch
das Deployment auf die verschiedenen Umgebungen bei jeder Codeänderung
durchgeführt. Das kann man nicht mehr manuell machen, dies muss man
automatisieren. Wenn etwas automatisiert ist, dann ist es dokumentiert und
wiederholbar. Wenn das Deployment auf die Pre-Produktionsumgebungen genau
so funktioniert, wie in die Produktion, dann ist das Vertrauen deutlich
höher, dass dies auch funktioniert. Generell versucht man hin zu einem
kontinuierlichen Fluss von wertschöpfenden Features in die Produktion zu
kommen.

![]({{ site.baseurl }}/assets/img/devops_und_itil/ContinuousDelivery_Prozesssicht.png)

Durch die kleinen Änderungen wird ein weiterer Effekt erzielt: Es gibt
mehr Standard Changes. ITIL unterscheidet zwischen Standard und Normal
Changes. Normal Changes müssen durch ein Change Advisory Board (CAB)
bewertet und freigegeben werden. Standard-Changes sind pre-approved und
werden als relativ risikoarm bewertet und folgen einem standardisierten
Prozess. Der standardisierte Prozess und die Risikoarmut werden durch die
Build-Pipeline mit kleinen Änderungen realisiert.

Gerade der letzte Punkt: Abgrenzung von Standard und Normal Changes und
das Vertrauen in die Buildpipeline kostet Überzeugungsarbeit. Es muss an
der Akzeptanz und der Definition der Changeklassen gearbeitet werden. Und
gerade in unserem Beratungsgeschäft gibt es wie schon beschrieben viele
Übergabepunkte zwischen Auftraggeber und z.B. dem Hoster.


IMPORTANT: Ab hier bis zum Ende überarbeiten

#### Incident Management #### Continuous Service Improvement

Continuous Service Improvement hat zum Ziel, die Effektivität und
Effizienz der IT-Services hinsichtlich der Geschäftsanforderungen
kontinuierlich zu verbessern.

*Service Measurement* Das Interesse der Fachbereiche fokussiert sich in
der Regel auf drei grundlegende Aspekte:

* Verfügbarkeit
* Zuverlässigkeit
* Performance

Bei der Gestaltung der Service-Messung sollte immer auf eine
End-To-End-Betrachtung der Services gesetzt werden. Einzelne Messgrößen
wie die Uptime einzelner Systeme sind zwar ein Teil der Lieferkette und
tragen zur Qualität bei, alleine betrachtet sind sie jedoch wenig
aussagekräftig. Kann ein Kunde zum Beispiel keine E-Mails mehr empfangen,
wird ihn wenig interessieren, dass der E-Mail Server gar keinen Ausfall
hatte und alles mal wieder am nicht verfügbaren Netzwerk lag. Im Rahmen
von DevOps stehen dabei 2 Aspekte im Vordergrund

* Measure Everything
* Information Radiation

Es nützt nichts, wenn Fehler nicht (schnell) analysiert werden können,
weil die notwendigen Informationen nicht oder nur schwer verfügbar sind
oder eine Korrelation von verschiedenen Informationen nicht möglich ist.
Erreicht wird eine schnelle Fehleranalyse, indem Informationen, die häufig
nur dem Ops Team zur Verfügung standen (etwa Log Daten) allen Mitgliedern
des Teams zur Verfügung stehen. Häufig wird dies durch zwei wesentliche
Technologien erreicht:

* Log Management
* Application Performance Monitoring (APM)

Beim Log Management geht es darum, Logs als Eventquellen zu betrachten und
zu zentralisieren. Logs sind häufig ein unterschätztes Mittel etwas über
den Zustand eines Systems zu erfahren, insbesondere, wenn diese einfach
abgefragt und korreliert werden können. Dabei unterstützt die Log
Management Software indem sie sich darum, Logs aus unterschiedlichen
Quellen zu zentralisieren, zu normalisieren, ggf. zu maskieren und einfach
abfragbar (ggf. abhängig von der Rolle) zu machen. Dies ermöglicht ohne
Zugriff auf die Systeme, auf ein wesentliches Werkzeug zuzugreifen.
Bekannte Vertreter sind Splunk (kommerziell) oder LogStash (Open Source).
Application Performance Manage wiederum setzt auf tieferen Ebenen an und
ist nicht auf die Generierung von Logs angewiesen. APM Software greift auf
Basis der JVM durch Instrumentierung des ausgeführten Codes Informationen
über den Verlauf einzelner Requests ab um folgende Fragen zu beantworten:

* Liegen Transaktionszeiten für einen Request im SLA Bereich?
* Wie ausgelastet sind verschiedene Pools (z.B. Thread oder Connection
  Pools)?
* Welche Komponente ist für die Dauer eines Requests hauptverantwortlich?

Diese Informationen sind wichtig, um Probleme zu analysieren oder
präventiv zu handeln. Dies kann durch die Anpassung der Infrastruktur an
den Bedarf erfolgen, um Sparpotenzial bei zu geringer Auslastung zu heben
oder rechtzeitige Bereitstellung von weiteren Ressourcen zu ermöglichen.
Ebenso ist die Anbindung von Monitoring und SLA Reporting sinnvoll, da
klassische Monitoring-Lösungen häufig nicht zuverlässig genug erkennen
können, ob eine Anwendung aus Anwendersicht nutzbar ist.

#### Service Asset & Configuration Management

### Fazit

