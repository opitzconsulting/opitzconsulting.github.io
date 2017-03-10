---
title: "Der Mythos der trunk-basierten Entwicklung"
author: richard.attermeyer
categories:
  - "continuous delivery"
tags:
  - DevOps
  - Continuous Delivery
  - Configuration-Management
  - Source Control
---

Langlebige Branches sind out - es lebe die trunk-basierte Entwicklung. So oder ähnlich lautet ein Mantra im Bereich Continuous Delivery.
Und ich glaube zu recht. [Paul Hammant][hamm2013] hat sehr gut erklärt, was trunk-basierte Entwicklung ist.
Dieses Branching-Modell wird aus verschiedenen Gründen gewählt.

## Merge Hell
Lang lebende Entwicklungs-Branches divergieren schnell und wenn sie wieder mit dem trunk zusammengeführt werden sollen, kommt es zur Merge-Hölle durch viele Konflikte, die erst einmal aufgelöst werden müssen.

## Keine verlässlichen Fortschrittsaussagen
In einem agilen Entwicklungsprozess können zwar alle Stories bearbeitet und für sich isoliert erledigt sein, aber ob das auch zu einem auslieferbaren Produktinkrement im Sprint führt ist nicht zwingend sichergestellt.
Ich habe schon in vielen Sprints erlebt in denen zwei Tage vor Sprintende die Entwickler sagten: "Wir müssen nur noch mergen". Und der Product Owner zitterte, ob überhaupt etwas auslieferbar ist, denn die Entwickler waren die letzten zwei Tage des Sprints mit dem Zusammenführen der Branches beschäftigt.

## Ausweg
Als ein Ausweg wird vorgeschlagen, dass die Entwicklung hauptsächlich auf dem Hauptzweig (trunk) stattfinden soll.

Viele Entwickler werden einwenden, dass es sogenannte breaking-changes gibt, die dazu führen, dass der trunk über lange Zeit nicht mehr erfolgreich bauen kann, etwa wenn ein querschnittliches Konzept geändert wird.

Auch dazu gibt es schon erprobte Muster, wie

* [Branch-by-Abstraction](http://martinfowler.com/bliki/BranchByAbstraction.html)
* [Feature Toggles](http://martinfowler.com/bliki/FeatureToggle.html)

Dann ist ja alles gut, oder?

## Probleme
Wenn direkt auf den trunk committed wird, besteht eine große Gefahr, dass der Trunk häufiger rot als grün ist.
Dieses Problem verschärft sich im Umfeld von Continuous Delivery. Im Rahmen einer Pipeline werden
verschiedene Tests durchgeführt. Unit Tests laufen schnell und die kann jeder Entwickler noch einfach auf seinem Entwicklungsrechner durchführen, aber schon bei Integrationstests sieht das anders aus.
Selbst bei einem mittelgroßen Projekt laufen Integrations- und Akzeptanztests schnell 30 Minuten und länger.

## Lösungsmöglichkeiten
Aus meiner Erfahrung wird das Muster "Entwicklung auf dem Hauptzweig" häufig falsch verstanden. Schon [Paul Hammant][hamm2013] schreibt in einem kleinen Satz:

> "More sophisticated companies will use pre-commit verifications."

Dies deutet zeigt, dass nicht alle Entwickler direkt ohne Prüfung des Codes auf dem Hauptzweig arbeiten. Das widerspricht auch nicht dem Sinn und Zweck der Entwicklung auf dem Hauptzweig.
Die Motivation für trunk-basierte Entwicklung liegt darin, lang laufende Branches zu vermeiden, häufig zu integrieren und Änderungen im Hauptzweig schnell in die Branches zu integrieren, an denen aktiv entwickelt wird.

Pre-Commit Verification kann dabei unterschiedliche Gesichter haben. Viele Entwickler haben vielleicht schon von [Gerrit][gerrit] gehört, einem Code Review System. Gerrit ermöglicht vor einem Push das Review von Codeänderungen.
Ein Review enthält dabei automatische und manuelle Anteile:

Automatisch wird ein Build auf einem CI Server angestoßen. Dabei wird überprüft, ob der Code sauber mit dem Hauptzweig zusammenzuführen ist und ob der aus dieser Zusammenführung resultierende Build grün ist. Grün bedeutet häufig, dass entsprechende statische Codeanalysen und Unit Tests erfolgreich waren.

Manuelle Prüfungen bedeutet, dass ein oder mehrere Kollegen im Peer Review die Codeänderung beurteilen.
Dass viele Projekte den Aufwand für ein manuelles Review scheuen, sollte aber nicht davon abhalten trotzdem ein automatisiertes Review durchzuführen.

Dies ist auch realisierbar über Jenkins Bordmittel, indem man eine Pre-Commit Pipeline einrichtet.
In einer Pre-Commit Pipeline wird der einzucheckende Code automatisch geprüft.
Das generelle Vorgehen ist dann in der Regel so, dass ein Entwickler nicht direkt auf den `trunk` committed, sondern auf einen `for/trunk`-Zweig. Ob dieser Zweig dann `story/for/trunk`, `team/for/trunk` oder `entwickler/for/trunk` lautet ist dann egal, da die `for-trunk-Pipeline` per Konvention alle Branches baut, die dem Muster `\*/for/trunk` entsprechen.

Folgende Schritte werden dann häufig in dieser for/trunk Pipeline ausgeführt

* Merge des aktuellen trunk auf den Branch
* Bauen des Branches
* Durchführen von Unit und Teilen der Integrations- und Akzeptanztests
* Merge des Branches in den trunk

Ziel dabei ist es nicht, dass der trunk nie wieder rot werden darf, sondern die Reduktion der Fälle in denen dies passiert. Man kann über die Anzahl der Testtiefe dieses Risiko steuern und ausbalancieren mit der Anforderung möglichst schnelles Feedback zu generieren.

In einem aktuellen Projekt bei dem vier Teams nach diesem Muster arbeiten, gibt es in der Regel ein bis
zweimal die Stunde einen Merge in den trunk.

## Weitere Informationen
Die Beschreibung, wie man einen Pre-Commit Build oder eine Pre-Commit Pipeline einrichtet, ist für einen Blogpost zu umfangreich.
Allerdings habe ich das Thema auch schon in einigen Artikeln verarbeitet, etwa auf [JAXenter](https://jaxenter.de/zeig-mir-deinen-code-396).
In einem zukünftigen Artikel möchte ich mich damit auseinandersetzen, was man für eine Pre-Commit Pipeline eventuell noch bedenken muss.

[hamm2013]: http://paulhammant.com/2013/04/05/what-is-trunk-based-development/
[gerrit]: https://www.gerritcodereview.com/
