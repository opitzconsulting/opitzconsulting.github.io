---
layout: page
show_meta: false
title: "Agilität"
subheadline: ""
permalink: "/agilitaet/"
header:
  - title: "Agilität"
---
<ul>
    {% for post in site.categories.agilitaet %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
