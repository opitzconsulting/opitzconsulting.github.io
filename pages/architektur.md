---
layout: page
show_meta: false
title: "Software- und Systemarchitektur"
subheadline: "Konzepte, Reviews und technische Lösungen"
permalink: "/architektur/"
header: 
  - title: "Software- und Systemarchitektur"
---
<ul>
    {% for post in site.categories.architektur %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
