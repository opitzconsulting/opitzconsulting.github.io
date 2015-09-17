---
layout: page
show_meta: false
title: "Automatisierung"
teaser: "Vom Commit einer Sourcecode-Änderung bis zum Release eines Software-Systems in Produktion ist es meist ein weiter Weg: Änderungen müssen getestet, Konfigurationen geändert, Patches eingespielt und Datenbankschemata aktualisiert werden. Das Ergebnis sind lange Releasezyklen und \"Bauchschmerzen\" vorm nächsten Release. Gleichzeitig steigen die Erwartungen Ihrer Fachanwender. Unsere Experten helfen die letzte Meile zu bewältigen und zeigen Ihnen die richtigen Praktiken und Werzeuge, um Deplyoment-Prozesse zu automatisieren und ein effektives Monitoring von Anwendungen und Systemen sicherzustellen."
subheadline: "Continuous Delivery, DevOps, APM, Logging"
permalink: "/automatisierung/"
header:
  - title: "Automatisierung"
---
<ul>
    {% for post in site.categories.automatisierung %}
    <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
