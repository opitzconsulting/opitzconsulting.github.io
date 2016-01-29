---
layout: page
permalink: /authors/index.html
title: Autoren
---

Auf dieser Seite möchten wir Ihnen unsere in diesem Blog aktiven Mitarbeiter und Autoren vorstellen:

<ul>
{% assign authors = site.authors  | sort: 'name' %}
{% for author in authors %}
	<li><a href="{{ site.baseurl }}{{author.url}}">{{author.name}}</a></li>
{% endfor %}
</ul>
