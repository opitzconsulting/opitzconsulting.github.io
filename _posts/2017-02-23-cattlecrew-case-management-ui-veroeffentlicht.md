---
title: CattleCrew Case Management UI veröffentlicht
date: 2017-02-23 00:00:00 Z
categories:
- BPM
tags:
- Case Mangement
- CMMN
- Camunda BPM
author:
- halil.hancioglu
---

Resultierend aus unserer bisherigen Case Management Erfahrung haben wir vor einiger Zeit eine 
selbst entwickelte Case Management UI auf GitHub veröffentlicht, um einen besseren Einstieg in 
Case Management und CMMN 1.1 zu ermöglichen. 

![Intro](/img/posts/2017-02-23/cccmui-intro.png)

Case Management hat sich neben dem traditionellen Prozess und Rules Management als drittes 
Standbein im BPM-Standardbaukasten etabliert. Dabei eignet sich Case Management besonders für 
dynamische Geschäftsprozesse, die eine hohe Varianz im Ablauf aufweisen. Was BPMN für 
traditionelle Prozessmodelle ist, ist der CMMN-Standard für Case Management.

Die CattleCrew Case Management UI ist aus der Idee entstanden, den Interessenten einen leichten 
Einstieg in das Thema durch ein geeignetes Tool zu ermöglichen. Wir haben uns für eine Benutzeroberfläche 
entschieden, weil wir während der ersten Gehversuche gemerkt haben, dass eine optimal zugeschnittene 
Benutzeroberfläche ein zentrales Element für Case Management ist. Um das Thema auch mithilfe der 
Community voranzutreiben, haben wir das Projekt auf GitHub veröffentlicht.

## Try it out
Nach dem befolgen der [Anleitung](https://github.com/opitzconsulting/cattlecrew-case-management-ui#try-it-out) auf GitHub und dem Aufrufen der Oberfläche öffnet sich die 
Übersichtsseite. Durch Auswahl eines Cases gelangt man auf die Detailansicht.

![Dashboard](/img/posts/2017-02-23/cccmui-dashboard.png)

In der Detailansicht werden alle Kontextinformationen zum aktuellen Case in vier Spalten angezeigt, 
um den Knowledge Worker bei seiner Arbeit zu unterstützen. Dies umfasst die historisierten Daten, 
Informationen aus vergleichbaren oder verwandten Fällen und bisher existierende Dokumente.

In der ersten Spalte von Links ist die Navigation und Suche von ähnlichen oder verwandten Cases platziert. 
Darunter werden alle zugehörigen Dokumente angezeigt. In der zweiten Spalte von Links 
sind die relevanten Kontextinformationen aufgelistet. Aus der dritten Spalte kann man erkennen, in 
welchem Status sich der Case gerade befindet und was bisher geschah. Die rechte Spalte zeigt die 
Aktivitäten an, die ausgeführt werden dürfen oder aktuell in Bearbeitung sind.

![Case Details](/img/posts/2017-02-23/cccmui-caseDetails.png)

Unter dem zweiten Tab-Reiter wird das gerenderte Case Modell angezeigt.

![Case Model](/img/posts/2017-02-23/cccmui-caseModel.png)

Unter dem dritten Tab-Reiter werden DMN-Tabellen mit den Eingabe- und Ausgabeparametern 
angezeigt. In der aktuellen Version allerdings nur, falls ein BPMN-Prozess aus dem Case gestartet 
wurde, welches eine DMN-Tabelle referenziert.

![Decision History](/img/posts/2017-02-23/cccmui-caseDetailsDecisionHistory.PNG)

Der vierte Tab-Reiter zeigt die ermittelten Daten an und ist speziell hilfreich für den Entwickler.

![Raw Data](/img/posts/2017-02-23/cccmui-caseDetailsRawData.PNG)

Durch Auswahl des „New Case“ Buttons in der Hauptnavigation kann ein neuer Case gestartet werden.

![New Case](/img/posts/2017-02-23/cccmui-newCase.png)

Es werden alle in der Engine existierenden Case Definitionen auf der linken Seite aufgelistet. 
Nach der Auswahl einer Case Definition können Prozessvariablen und ein fachlicher Schlüssel zum Case vor dem Start hinzugefügt werden.

![New Case With Data](/img/posts/2017-02-23/cccmui-newCaseWithVariable.png)

Sobald ein Case gestartet wurde, wird man zur Übersichtsseite geleitet. Hier ist dann auch der gestartete 
Case sichtbar.

## Eingesetzte Process Engine
Das veröffentlichte Projekt setzt die Camunda BPM Process Engine voraus. Camunda BPM ist einer 
der ersten Softwarehersteller die CMMN 1.1 in Ihre Process Engine integriert hat und auch einen 
entsprechenden Modeler zur Verfügung stellt. Seit Camunda 7.6 ist auch das Feature „CMMN Monitoring“ 
in der Enterprise Version enthalten.

CattleCrew Case Management UI ist so aufgebaut, dass die Engine spezifische Implementierung durch 
eine andere ersetzt werden kann, um die UI auch mit anderen Engines zu erproben. In der 
nachfolgenden Abbildung sind die einzelnen Schichten abstrakt dargestellt.

![Bird View](/img/posts/2017-02-23/cccmui-architectureOverview.png)

Das Projekt ist auf GitHub unter [https://github.com/opitzconsulting/cattlecrew-case-management-ui/](https://github.com/opitzconsulting/cattlecrew-case-management-ui/) 
zu finden. Dort liegt auch eine Installationsanleitung bei. Der QR-Code ist

![QR Code](/img/posts/2017-02-23/cccmui-qrCode.png)

Ihr seid herzlichst eingeladen, um an dem Projekt mitzuwirken. Kommt bei Interesse einfach auf uns zu. 

Viel Spaß beim Experimentieren mit Case Management und CMMN!

Feedback ist gerne willkommen ;-)
