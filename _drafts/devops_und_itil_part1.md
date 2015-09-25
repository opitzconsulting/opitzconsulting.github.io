--- 
layout: page
header: no
title: "DevOps und ITIL: Waffenbrüder oder Feinde? (Teil 1)"
teaser: "DevOps und ITIL scheinen auf einen ersten oberflächlichen Blick unterschiedliche Ziele zu verfolgen. Die Wahrnehmung von ITIL ist, dass es eine Methode ist, die häufig in größeren Organisationen zum Einsatz kommt in denen Entwicklung und Betrieb stark voneinander getrennt arbeiten und dies auch so gewollt ist. DevOps aber wird häufig mit dem Einreißen der Mauern zwischen diesen
Organisationseinheiten in Verbindung gebracht. In dieser Blogserie zeigen wir, wie beide zusammen funktionieren können"
author: richard.attermeyer
categories:
- automatisierung
- agilität
---

DevOps und ITIL scheinen auf einen ersten oberflächlichen Blick unterschiedliche Ziele zu verfolgen. Die Wahrnehmung von ITIL ist, dass es eine Methode ist, die häufig in größeren Organisationen zum Einsatz kommt in denen Entwicklung und Betrieb stark voneinander getrennt arbeiten und dies auch so gewollt ist. DevOps aber wird häufig mit dem Einreißen der Mauern zwischen diesen
Organisationseinheiten in Verbindung gebracht. In dieser Blogserie zeigen wir, wie beide zusammen funktionieren können

## Wie OPITZ CONSULTING Software entwickelt

Kurz zusammengefasst: Im Idealfall agil, in kurzen Iterationen und eng
zusammen mit dem Kunden. Dies bedeutet auch, dass wir gerne klassische
Festpreisprojekte vermeiden. Dort wo Umfang und Kosten festliegen und
wird häufig an der Qualität gespart. Denn selbst wenn die Planung auf
Basis des aktuellen Wissens korrekt ist, lernt man während der
Projektlaufzeit häufig über die Fachlichkeit und die Technik hinzu und
es ergeben sich Änderungen. Sparen an der Qualität ist für uns
normalerweise keine Option footnote:[Außer der Kunde hat das bei der
Erhebung der Qualtitäsattribute vorher explizit dargestellt und durch
Szenarien konkretisiert, an welcher Qualität gespart werden darf.].
Handwerklich solide erstellte Software senkt die Lebenszykluskosten der
Software, davon sind wir überzeugt.

Häufig wird in der Softwareentwicklung ein Fokus auf die externe
Qualität gelegt. Tester betrachten Software als eine Black-Box und
prüfen, ob sie das erwartete leistet. Dies ist aber insbesondere
hinsichtlich des Investitionsschutzes zu kurz gedacht. Dort ist die
interne Qualität von Software genauso wichtig. Software wird viel
häufiger gelesen als geschrieben. Betracht man den Lebenszyklus von
Applikationen, so fällt der größte Anteil an Kosten für Software in der
Wartungsphase an. Eine zentrale Anforderung an eine neu zu entwickelnde
Software sollte also lauten: „Die Software muss wartbar sein“. Software,
die einfach zu verstehen und zu restrukturieren ist, ist günstiger in
der Wartung. Daher soll Software nicht nur funktionieren, sondern Sie
soll auch gut gefertigt sein. Um die Wartbarkeit sicher zu stellen,
müssen Betrieb und Entwicklung schon bei der Realisierung eng
zusammenarbeiten.

Die agilen Prinzipien und Werte sind ein Grundpfeiler unserer
Projektarbeit. Dabei wenden wir häufig Scrum als
Projektmanagementrahmenwerk an. Wir entwickeln in kurzen Iterationen
(Sprints), schreiben unsere Anfoderungen und Aufgaben als User Stories
und Tasks auf und dokumentieren notwendige Spezifikationen in einem
Wiki. Das wiederum möglichst dem Prinzip Specification-by-Example
folgend. Dabei legen wir viel Wert auf das Erledigen der Aufgaben in
weitestgehend selbstorganisierten Teams. In Zusammenarbeit mit dem
Product Owner wird die Maximierung des Geschäftswertes dann
sichergestellt. Die besonderen Herausforderungen entstehen, wenn ein
Projekt von der Entwicklung in den Regelbetrieb übergeht. Für die
Softwareweiterentwicklung und den -support stellen sich dann einige
(exemplareische) Fragen:

* Betreuen ggf. andere Kollegen die Software als diejenigen, die sie
entwickelt haben?
* Wie erfolgt der Wissenstransfer?
** Technologisch
** fachlich
* Kann ein Wartungsteam die Anwendung zusammen mit anderen Anwendungen
betreuen?

![Prinzipien der Entwicklung]({{ site.baseurl }}/assets/img/devops_und_itil/Entwicklung.png)

## DevOps als Brückenschlag

DevOps ist eine Philosophie, welches sich mit der Entwicklung, dem Betrieb
und den Dienstleistungen um Softwareentwicklung herum beschäftigt. Es
legt Wert auf Zusammenarbeit und die Integration von Softwarentwicklung
und Betrieb. Der DevOps Ansatz zielt darauf ab, dass Organisationen
schnell Softwareprodukte und Services in einer hohen Qualität entwickeln
und bereit stellen können. Dabei ist DevOps keine Methode, welche vorschreibt,
wie die Ziele zu erreichen sind.

Hohe Qualität heißt, dass weniger Releases fehlschlagen und weniger Zeit
für die Wiederherstellung benötigt wird, wenn ein Release fehlschlägt;
die Lead Time zwischen Bug Fix und Bereitstellung verkürzt wird. DevOps
soll die Vorhersagbarkeit, Effizienz, Sicherheit und die Einhaltung von
Betriebsprozessen verbessern. Sehr häufig wird dies durch eine
Automatisierung von Prozessen erreicht. Viele der Ideen innerhalb von
DevOps kommen aus dem Bereich Enterprise Systems Management und agiler
Softwareentwicklung und sind eine Antwort auf aktuelle Trends und Herausforderungen
an die IT-Abteilungen von Unternehmen.

## Trends und Herausforderungen in der Softwareentwicklung

In den letzten Jahren haben sich agile Projektmethoden, wie Scrum, Kanban, FDD
als Standardmethoden für innovative Softwareprojekte etabliert. Innovative
Softwareprojekte zeichnen sich häufig dadurch aus, dass die Anforderungen zum
Teil unklar sind, gleichzeitig aber die Produkteinführung über kurze Zeitdauern
realisiert werden sollen.

Produkte in immer mehr Industrie- und Dienstleistungsbereichen sind stark durch
Software getrieben. Hier trägt insbesondere die Digitalisierung vieler
Lebensbereiche und der Industrieproduktion, Stichwort: Industrie 4.0 und
Internet of Things (IoT), dazu bei, dass IT als Teil der Wertschöpfungskette
gesehen wird und nicht nur als Kostenfaktor.

Gleichzeitig werden aber häufig die Zeitfenster für eine
erfolgreiche Produktplatzierung kleiner. Agile Methoden entwickeln zwar häufig
in kurzen Iterationen (etwa 2-3 Wochen), aber bis ein Produktinkrement wirklich
den Weg bis zum Endkunden gefunden hat, vergeht leicht drei bis viermal so viel
Zeit (4-12 Wochen). Zudem würden die Fachbereiche oft gerne häufiger neue
Produkte am Markt platzieren. Denn auch dort setzen sich Gedanken von agilen
Methoden und Lean Management durch. Diese setzen das Experiment und kurze
Feedbackzyklen für die Weiterentwicklung eines Geschäftsbereichs ein.

Diese Idee passt auch zu der Unterteilung der Unternehmensarchitektur in
Applikationen mit unterschiedlich langen Entwicklungs- und Releasezyklen
(vergleiche <<gartner>>). Für die Systems of Innovation reicht eine agile
Entwicklungsmethodik nicht aus, um die kurzen Durchlaufzeiten von der Idee bis
zur Produktion zu erreichen. Eine Fortführung agiler Entwicklungspraktiken
möglichst bis zur Produktion ist notwendig. Dies ist bekannt unter dem Namen
DevOps oder Continuous Delivery / Continuous Deployment.

DevOps selber adressiert das Problem vieler Unternehmen, dass Entwicklung und
Betrieb stark voneinander getrennte Einheiten darstellen. An der Schnittstelle
kommt es daher häufig zu Reibungsverlusten. Gleichzeitig scheinen sich die
Zielvorgaben von Entwicklung und Betrieb zu widersprechen: Die Entwicklung wird
an der Menge umgesetzter Anforderungen gemessen, der Betrieb an der Stabilität
der Produktionsumgebung. 

DevOps selber ist eine Philosphie, um Betrieb und Entwicklung wieder näher
zueinander rücken zu lassen. Beide sind gemeinsam verantwortlich möglichst den
Unternehmenserfolg zu steigern, indem mehr geschäftsrelevante Anforderungen in
kurzer Zeit bereit gestellt werden. DevOps vertritt eine system-orientierte
Sichtweise. Nicht die Optimierung eines Bereichs (Entwicklung oder Betrieb)
oder einer einzelnen Wertschöpfungskette steht im Fordergrund, sondern es
werden alle Wertschöpfungsketten betrachtet, die durch IT unterstützt werden.
Dabei wird ein Fokus auf den Flaschenhals im Prozess gelegt (<<goldratt>>).

## Notwendigkeit der Standardisierung von Prozessen

OPITZ CONSULTING als Beratungsunternehmen steht im Rahmen der Softwareentwicklung im
Spannungsfeld Projektarbeit und Wartung. Die Spannung entsteht aus verschiedenen
Gründen. Es gibt zum einen klassische Werkverträge für die Gewährleistung übernommen wird und
Fehlerkorrekturen durchgeführt werden müssen. Ein weiterer Grund ist Software, für die
Unternehmen Weiterentwicklung und Betreuung einkaufen wollen. Häufig entsteht dort über ein ganzes
Jahr verteilt immer wieder Aufwand, aber häufig rechnet sich ein Projektteam nicht. 

Klassisch gibt es zwei Lösungsmöglichkeiten: 

* Dedizierte Wartungsteams
* Produktteams, die auch Wartung übernehmen 

Für dedizierte Wartungsteams spricht, dass Wartungsaufgaben und Projektaufgaben
klar getrennt sind und sich nicht gegenseitig beeinflussen.

Produktteams, die auch Wartung übernehmen, haben häufig das Problem
keine verlässlichen Aussagen über Projekttermine und den Scope machen zu können.

Dies ist insbesondere dann problematisch, wenn andere Teams von diesen
Ergebnissen abhängen. Gegen dedizierte Wartungsteams spricht, dass unter
Umständen mehr Personal benötigt wird oder dass es zu mehr Personalfluktuation
in Wartungsteams kommt, da Wartung schwierig ist und keine "modernen" Techniken
zum Einsatz kommen - ein nicht zu unterschätzender Motivator für viele junge
Entwickler.

Im Folgenden gehen wir von einem dedizierten Wartungsteam aus.
Wartungs- und Projektteams bedeutet nicht automatisch mehr Personal, wenn die
verwendeten Techniken ähnlich sind. Etwa ein Wartungsteam für alle Java
Projekte. Dies ist sinnvoll, wenn genügend Wartung im Bereich anfällt, um ein
Team (d.h. mehr als 3 Leute) dauerhaft auszulasten.

Es ist wichtig, dass es ein Team ist, denn als Wartungsteam müssen häufig
Reaktionszeiten, wie sie in SLAs definiert sind eingehalten werden. Dies geht
nur, wenn im Team ausreichend Mitarbeiter zur Verfügung stehen, um Urlaub
Krankheit Personalfluktuation und Lastspitzen auffangen zu können.

Wenn ein Team für alle Java Projekte zuständig ist, dann
kommt es häufig vor, dass ein Kollege für mehrere Projekte und interne /
externe Kunden zuständig ist.

## Managed Services bei OPITZ CONSULTING
Aus diesen Überlegungen heraus haben wir unser Leistungsportfolio um Managed Services
Dienstleistungen erweitert. Wir differenzieren dabei zwischen 

* Managed Services Applications (MSA)
* Managed Services Infrastructure (MSI)

MSA kümmert sich dabei um den Support und die Weiterentwicklung von Applikationen. 
MSI wiederum bietet die Überwachung und den Betrieb von Datenbanken, Middleware und Betriebssystemen.

Häufig werden diese Dienstleistungen getrennt voneinander angefragt. Die Probleme bei der funktionalen
Trennung dieser Dienstleistungen werden wir später noch einmal diskutieren.
Im Folgenden geht es aber mehr um den Bereich Managed Services Applikationen.

![Prinzipien der Entwicklung]({{ site.baseurl }}/assets/img/devops_und_itil/ManagedServices_Aufgaben.png)
 
## ITIL als Service Enabler

Unterschiedliche Kunden, Projekte und die Notwendigkeit von Vertretungsregelungen bei gleichbleibender 
Servicequalität sind Themen, die man aus dem „klassischen“ IT-Betrieb kennt. Solche
Situationen rufen häufig den Wunsch nach Standardisierung von Prozessen hervor.
In diesem Zusammenhang ist es selten sinnvoll neue und einmalige Prozesse zu
erfinden. Stattdessen ist es hier eine gute Idee Industriestandards zu
verwenden und diese ggf. anzupassen. Für den Bereich des IT Betriebs hat sich
in den letzten Jahren ITIL als ein Prozesskatalog etabliert, um IT Services
standardisiert zu betreiben. In diesem Sinn stellt ITIL eine Grundlage dar, um
Softwarewartung als Dienstleistung zu erbringen.

Ein paar der Herausforderungen wurden schon angedeutet. Ziel ist es
wesentliche Prozesse zu vereinheitlichen, so dass Mitarbeiter Wartungsprojekte
mehrerer Kunden (extern und intern) betreuen können und dies in einer guten
Qualität. Fehler sollen vermieden werden, indem Abläufe standardisiert werden
wo notwendig und sinnvoll ohne aber individuelle, kundenspezifische Anpassungen
zu verhindern.

ITIL definiert eine Menge an Prozessen, von denen wir uns in
diesem Whitepaper auf folgende Bereiche konzentrieren, die im Zusammenhang mit
DevOps von besonderem Interesse sind.

* Service Transition
* Service Operation
* Continual Service Improvement

Allerdings steht ITIL im Ruf schwerfällig zu sein
und Anforderungen, die an die Entwicklung gestellt werden nicht zu
berücksichtigen oder gar deren Umsetzung zu verhindern. Aus diesem Grund sind
Entwickler häufig sehr skeptisch gegenüber ITIL Prozessen eingestellt.

