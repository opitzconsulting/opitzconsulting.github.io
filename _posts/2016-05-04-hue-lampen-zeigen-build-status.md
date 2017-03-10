---
title: "Anzeige des Jenkins Buildstatus mit Hue-Lampen"
author: marco.buss
categories:
  - continuous delivery
tags:
  - DevOps
  - Continuous Delivery
---

In vielen Teams hat sich der Buildmonitor von Jenkins zur Überwachung der Buildjobs etabliert. Einfach einen Monitor an einer entsprechende Stelle aufstellen, Jenkins konfigurieren und schon hat der geneigte Entwickler alles im Blick.

![Beispielhafter Buildmonitor](/img/posts/2016-05-04/build_monitor.jpg)

In einem aktuellen Projekt konnte aber nicht jeder Entwickler direkt auf den Buildmonitor schauen. Mehrere Monitore waren im speziellen Fall nicht möglich,
also musste eine Alternative gefunden werden.

Glücklicherweise waren aus der Anfangszeit noch 2 Hue Lampen von Phillips vorhanden. Mit diesen Lampen wurde
in der Anfangsphase des Projektes der Buildstatus visualisert. Jenkins bietet hierfür sogar ein Plugin an. Der Nachteil des Plugins ist allerdings, daß pro Lampe lediglich
ein Buildjob visualisiert werden kann. Dadurch kann nicht der Gesamtstatus einer Buildqueue visulaisiert werden.
Die Lampen lassen sich sehr einfach über eine Rest-Schnittstelle ansteueren. Jenkins bietet
ebenfalls eine Restschnittstelle, es liegt also auf der Hand beide Möglichkeiten zu nutzen um den Zustand mehrerer Builds darzustellen.

## Konfiguration des Jenkins
Auf dem Jenkins muss eine Buildmonitor-View eingerichtet werden. In diese Buildmonitor-View werden dann alle Projekte aufgenommen die es zu überwachen gilt.
Dabei können die Jobs entweder einzeln ausgewählt oder über einen regulären Ausdruck bestimmt werden.

![Anlegen des Buildmonitors](/img/posts/2016-05-04/jenkins-config-1.jpg)

Ist die View angelegt, ist sie über eine URL erreichbar. Diese View kann nun auch als JSON Repräsentation dargestellt werden. Dazu muss lediglich der
Präfix `/api/json?pretty=true` an die URL angefügt werden.

## Konfiguration der HUE Schnittstelle
Die Hue Schnittstelle benötigt keine weitere Konfiguration. Es muss einfach der folgenden Anleitung gefolgt werden um einen neuen Nutzer anzulegen.
[http://www.developers.meethue.com/documentation/getting-started](http://www.developers.meethue.com/documentation/getting-started)

Der Nutzer wird dann benötigt um die entsprechenden Befehle an die HUE Bridge zu senden.

## Shell Script
Das nachfolgende Shellscript verbindet am Ende beide Schnittstellen miteinander. Das Script muss lediglich als Cron Job eingerichtet werden.
Nachdem alles eingerichtet ist, müssen nur noch die Lampen entsprechend verteilt werden und jeder hat den Status der Buildkette im Blick. Auch ohne
direkten Blickkontakt zum Buildmonitor.
 
{% highlight sh %}
#!/bin/bash

# Hier wird der Jenkins abgefragt, ob Jobs im Status 'gelb' oder 'rot' sind
status_yellow=$(curl "<URL zum Buildmonitor>/api/json?pretty=true" | grep -om 1 "yellow")
status_red=$(curl "<URL zum Buildmonitor>/api/json?pretty=true" | grep -om 1 "red")

status_yellow=x$status_yellow
status_red=x$status_red

# Damit werden die einzelnen Lampen in der entsprechenden Farbe aktiviert
if [[ $status_red == "xred" ]]
then
    echo "Mark red"
    curl -X PUT -d '{"on":true, "sat":254, "bri":254, "hue":0}' http://192.168.2.197/api/newdeveloper/lights/1/state
    curl -X PUT -d '{"on":true, "sat":254, "bri":254, "hue":0}' http://192.168.2.197/api/newdeveloper/lights/2/state
elif [[ $status_yellow == "xyellow" ]]
then
    echo "Mark yellow"
    curl -X PUT -d '{"on":true, "sat":254, "bri":254, "hue":10000}' http://192.168.2.197/api/newdeveloper/lights/1/state
    curl -X PUT -d '{"on":true, "sat":254, "bri":254, "hue":10000}' http://192.168.2.197/api/newdeveloper/lights/2/state
else
    echo "Mark green"
    curl -X PUT -d '{"on":true, "sat":254, "bri":254, "hue":30000}' http://192.168.2.197/api/newdeveloper/lights/1/state
    curl -X PUT -d '{"on":true, "sat":254, "bri":254, "hue":30000}' http://192.168.2.197/api/newdeveloper/lights/2/state
fi

{% endhighlight %}
