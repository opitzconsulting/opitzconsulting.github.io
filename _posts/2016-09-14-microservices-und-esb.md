---
title: "Microservices Architektur und ESB: Unvereinbar?"
author: richard.attermeyer
categories:
  - architektur
tags:
  - Microservices
  - Flexible Architekturen
  - SOA
  - Integration
---
"Passt ein Enterprise Service Bus in eine Microservices Architektur", das fragen sich viele, die bisher auf eine Service-orientierte Architektur gesetzt haben.

## Der Enterprise Service Bus
Wird ein ESB in eine Service-Orientierten-Architektur eingesetzt, dann übernimmt er zentrale Aufgaben, wie:

* Routing
* Protokolltransformation
* Mapping zwischen Datenstrukturen
* Content Enrichment / Content Filtering
* Endpunktvirtualisierung

Anstatt bei dieser technischen Beschreibung zu bleiben, sollten wir überlegen, welche Architekturtreiber hinter der
Entscheidung stehen, einen ESB zu vewenden (wir klammern einmal aus, dass nicht klar ist, was ein ESB ist).

Der Einsatz eines ESBs wird häufig mit folgenden Architekturtreibern begründet:

* Effizienz
* Sicherheit
* Manageability

Dabei sollten wir nicht stehen bleiben und uns etwas weiter mit den Hintegründen beschäftigen, um zu verstehen, was mit den Schlagworten gemeint ist

### Effizienz
Bevor ein ESB für die Integration von Systemen verwendet wurde, hat man häufig Punkt-zu-Punkt Verbindungen aufgebaut.
Das Versprechen des ESBs ist ess, dass die jeweiligen Entwicklungsteams sich nicht um
Protokoll und Formatkonvertierung Gedanken machen und es implementieren müssen, sondern dass sich diese Aufgabe ein ESB / Middlewareteam übernimmt.
Hier kann sogar ein "Common Data Model" definiert werden, und dann braucht man ggf. nur eine neue Konvertierung vom CDM zum neuen angebundenen System implementieren.

### Manageability
Ein ESB Produkt ist Teil einer Middlewarelösung. Als solche ist es ein zentraler Punkt, an dem man recht einfach Richtlinien durchsetzen und kontrollieren kann, etwa bzgl. Sicherheit (Authentifizierung, Autorisierung, Auditing).
Daher ist ein ESB ein gutes Produkt, wenn man Enterprise Architektur Vorgaben immplementieren und kontrollieren will.
Den Auditor freut es auch häufig, wenn er sich nur ein Produkt anschauen muss, um zu verstehen, wie Sicherheitsaspekte adressiert werden.
Des Weiteren dient ein ESB als zentrale Architekturkomponente dazu, Nachrichten and verschiedene Endsysteme zu verteilen. Dabei werden Nachrichten in das jeweilige Zieldatenformat transformiert und ggf. auch um weitere Daten angereichert.

### Sicherheit (Security)

Insgesamt herrscht bei den Motiven hinter einem ESB und vielleicht auch hinter einer klassischen SOA ein Top-Down bzw. zentralistischer Ansatz vor. Vorgabe eine Common Data Models, zentraler Business Regeln, die zentral durchgesetzt und
gemonitored werden können. Sicherheit, die zentral und einheitlich gemanaged werden kann.

### SOA als zentralistischer Ansatz
SOA wird häufig als unternehmensweite Initiative eingeführt. Wenn das nicht passiert, dann werden nur Teilerfolge erzielt oder aus SOA wird einfach Enterprise Appplication Integraiton (EAI).
Bei solchen Ansätzen spielt dann auch Enterprise Architecture eine Rolle. Wobei EA dort wiederum als zentrale Vorgabe verstanden wird.
Effizienz wird dann auch so verstanden, dass wie bei einer arbeitsteiligen Industriefertigung eine gewisse Tätigkeit (etwa das Mapping zwischen Datenmodellen) am besten von einer Abteilung vorgenommen wird, die nichts anderes macht und es daher auch sehr effizient macht.
Was man aber dabei verliert ist Agilität. Wenn alle Prozesse auf die bestmögliche Auslastung der Mitarbeiter ausgelegt sind, dann ist man häufig nicht mehr in der Lage kurzfristig auf sich ändernde Rahmenbedingugen reagieren zu können.

Das ist aber einer der Gründe für Microservices.

## Microservices

Da der Begriff Microservices genau so wenig präzise definiert it, wie der Begriff ESB, betrachten wir für diesen Blog Post einen Microservices als einen Service, der im Idealfall für eine Geschäftsfähigkeit (Business Capability) zuständig ist. Weiter gehe ich davon aus, dass grundlegende Entscheidungen für die Kommunikation von Microservices untereinander
zentral festgelegt wurden, etwa REST/HTTP für synchrone Aufrufe, Messaging für asynchrone Kommunikation.

Routing wird entweder druch die Messaging Middleware erledigt oder die einzelnen Services finden sich über eine Service Registry.
In der SOA Welt wurden auch schon Themen wie Service Registries versucht zu etablieren. Aber zum einen war es nicht so nötig, da die Infrastruktur recht stabil war und zum anderen waren die Registries häufig zu kompliziert (Aufwand für Pflege und Nutzen stehen in keinem guten Verhältnis). Registries im Microservices Umfeld sind dichter dran an einem einfachen DNS Server, der ggf. dann noch Zusatzfunktionen bietet, wie Health-Checks etc.

Bleiben die Aspekte Sicherheit und Modelltransformation. Zunächst zur Modelltransformation. Wenn das MS Team selbst die Transformation durchführt, dann kennt es zumindest eines der Modelle und nicht wie das Middlewareteam beide nicht.
Der Aspekt der Sicherheit wiederum ist ein querschnittliches Thema. Daher ist es sinnvoll diese Aufgaben in einem API Gateway zu bündeln.
Dieser sollte aber keine Business- oder Transformationslogik implementieren. Wesentliche Aspekte beschränken sich auf Sicherheit und Monetarisierung.

Kurzum kann man sagen, dass in einer Microservices Architektur ein klassischer ESB, der häufig auch als ein Produkt / Plattform abgebildet wird ausgedient hat.

Also können wir den ESB auf den Dachboden verbannen, wo er verstaubt? Ich denke nicht.

## Legacy Systeme

Microservices entstehen nicht im luftleeren Raum. Es gibt Architekturtreiber oder Randbedingungen die sich häufig in Unternehmen finden:

* Integration mit Legacy Systemen (Cobol, 4GL, Java Monolith oder Kaufsoftware)
* Entwickler Know How: Entwickler der Legacy Systeme beherrschen ABAP, Cobol oder andere 4GL Sprachen und werden nicht so schnell Java, Typescript oder GO zusätzlich lernen

Diese Legacy Systeme unterscheiden sich in den Datenformaten (z.B. fixed Length, CSV) oder den Protokollen die sie sprechen (z.B. TCP, RPC-style SOAP) stark von den Kommunikationsprotokollen und -datenformaten der Microservices.

Trotzdem müssen die Microservices mit diesen zusammenarbeiten, um einen Geschäftswert zu liefern.

Es stellt sich also die Frage, wie die Integration am Besten funktionieren kann? Eine Antwort aus dem Domain-Driven-Design sind Anti-Corruption-Layer.

### Anti-Corruption-Layer als Alternative?
Die Bounded-Contexts der Microservices sind unabhängig voneinander (lassen wir mal den Shared Kernel außer Acht). Als unabhängige Einheiten werden sie auch weiterentwickelt. Diese Änderungen können dann dazu führen, dass Events sich ändern.
Ein anderer Microservice welcher diesen Event konsumiert, sollte robust gegenüber Änderungen an anderen bounded contexts sein.
Die Lösung dafür ist ein Anti Corruption Layer (ACL). Als solcher könnte der ACL sicherstellen, dass der Eventinhalt die benötigten Daten und
korrekten Typen enthält. Ebenso könnte ein ACL auch eine Übersetzung vornehmen. Dies betrifft hier auch das Protokoll.

Der ACL liegt in der Verantwortung des empfangenden Systems. Einen ACL in einer Legacy 4GL Sprache oder Cobol zu implementieren ist nicht einfach.

### ESB als Vermittler

Daher sehe ich selbst enwickelte ACLs nicht als Lösung an. Die Rolle eines ACLs kann aber recht gut von einem ESB übernommen werden.
Er stellt auf der einen Seite wohlgeschnitte "Business Services" bereit, welche keine Implementierungsdetails des zugrunde liegenden Systems
nach außen gibt (Attribute in der Nachricht sollten dann nicht SAP typisch "zz_" heißen). Bzgl. der Service Schnittstelle gelten für diese Services die gleichen Regeln wie für Microservices. Konzeptionell sollte das kein Unterschied machen.

Also kann man mittels ESB Legacy Systeme und Microservices integrieren.

![Legacy und Service Domäne verbunden](/img/posts/2016-09-14/CAFA.png)
Konkret kann das etwa wie unten dargestellt aussehen. Hier ruft ein API Gateway einen Businessservice
auf dem ESB auf, der von SAP implementiert wird. So ist dies aktuell bei einem
Kunden eines Kollegen implementiert.
![API GW spricht mit ESB](/img/posts/2016-09-14/api_gw_esb_beispiel.jpg)

In diesem Fall wird eine Bestellung über das API Gateway aufgegeben.

## Wieder vereint: ESB und Microservices?

Wir haben gesehen, dass es aufgrund von Architekturtreibern sinnvoll ist, einen ESB zur Integration von zwei Domänen mit unterschiedlichen
Architekturstilen zu verwenden.

Was wir bisher nicht betrachtet haben sind die organisatorischen Voraussetzung, unter denen die beiden Domänen im gleichen Unternehmen existieren können. Eine Microservicearchitektur in einer klassisch organisierten IT Organisation wird nicht funktionieren.
Die alte Organisationsstruktur ist natürlich auch mit Köpfen verknüpft; Organisationen und Menschen sind aber eine träge Masse.
Dies mag auch einer der Gründe sein, dass Unternehmen eigene Gesellschaften gründen, in denen sie ihre Aktivitäten rund um Digitalisierung bündeln. Diese Gesellschaften haben aber immer noch Integrationspunkte mit dem Mutterunternehmen, aber ihre ganze Aufbauorganisation ist auf die Herausforderungen der Digitalisierung ausgerichtet. Agilität, DevOps und Prinzipien von Management 3.0 sind in diesen Organisationen natürlicher Bestandteil ihrer DNA.
Viel wichtiger wäre es aber direkt mit einem Kultur- und damit Organisationswandel zu beginnen. Silodenken ist eine Sackgasse, um in sich schnell ändernden Märkten und Umgebungen erfolgreich zu sein.


## Dank
Ich danke meinem Kollegen [Sven Bernhardt](https://svenbernhardt.wordpress.com/) für wertvolle Kommentare zu einer frühen Version dieses Artikels.
