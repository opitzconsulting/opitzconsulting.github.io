---
title: "Microservices Architektur und ESB: Zwei Gegensätze?"
author: richard.attermeyer
categories:
  - Flexible Architekturen
tags:
  - Microservices
  - SOA
  - Integration
---
"Passt ein Enterprise Service Bus in eine Microservices Architektur", das fragen sich viele, die bisher auf eine Service-orientierte Architektur gesetzt haben.

## Der Enterprise Service Bus
Ein ESB übernimmt in einer SOA Architektur wichtige zentrale Aufgaben:

* Routing
* Protokolltransformation
* Mapping zwischen Datenstrukturen

Anstatt bei dieser technischen Beschreibung zu bleiben, sollten wir überlegen, welche Architekturtreiber hinter der
Entscheidung stehen, einen ESB zu vewenden (wir klammern einmal aus, dass nicht klar ist, was ein ESB ist).

Der Einsatz eines ESBs wird häufig mit folgenden Architekturtreibern begründet:

* Effizienz
* Sicherheit
* Manageability

Dabei sollten wir nicht stehen bleiben und uns etwas weiter mit den Hintegründen beschäftigen, um zu verstehen, was mit den Schlagworten gemeint ist

### Effizienz
### Manageability
### Sicherheit (Security)

Insgesamt herrscht bei den Motiven hinter einem ESB und vielleicht auch hinter einer klassischen SOA ein Top-Down bzw. zentralistischer Ansatz vor. Vorgabe eine Common Data Models, zentraler Business Regeln, die zentral durchgesetzt und
gemonitored werden können. Sicherheit, die zentral und einheitlich gemanaged werden kann.
SOA wird häufig als unternehmensweite Initiative eingeführt. Wenn das nicht passiert, dann werden nur Teilerfolge erzielt oder aus SOA wird einfach Enterprise Appplication Integraiton (EAI).
Bei solchen Ansätzen spielt dann auch Enterprise Architecture eine Rolle. Wobei EA dort wiederum als zentrale Vorgabe verstanden wird.
Effizienz wird dann auch so verstanden, dass wie bei einer arbeitsteiligen Industriefertigung eine gewisse Tätigkeit (etwa das Mapping zwischen Datenmodellen) am besten von einer Abteilung vorgenommen wird, die nichts anderes macht und es daher auch sehr effizient macht.
Was man aber dabei verliert ist Agilität. Wenn alle Prozesse auf die bestmögliche Auslastung der Mitarbeiter ausgelegt sind, dann ist man häufig nicht mehr in der Lage kurzfristig auf sich ändernde Rahmenbedingugen reagieren zu können.

Das ist aber einer der Gründe für Microservices

## Microservices

....


Kurzum kann man sagen, dass in einer Microservices Architektur ein klassischer ESB, der häufig auch als ein Produkt abgebildet wird ausgedient hat. In einer abgespeckten Form findet er sich als API Gateway wieder.
Dieser sollte aber keine Businesslogik implementieren. Wesentliche Aspekte beschränken sich auf Sicherheit und Monetarisierung.

Also können wir den ESB auf den Dachboden verbannen, wo er verstaubt?

## Legacy Systeme

MS entstehen nicht im luftleeren Raum. Es gibt Architekturtreiber, die hier eher Randbedingungen bedeuten

* Integration mit Legacy Systemen
* Entwickler der Legacy Systeme beherrschen ABAP, Cobol oder andere 4GL Sprachen und werden nicht so schnell Java, Typescript oder GO zusätzlich lernen
*
Nur Startups habe keine Legacy Systeme.

### ACLs als Alternative?

Nein, tragen aus meiner sicht nicht
