---
layout: page
show_meta: false
title: "Softwaremodernisierung"
subheadline: "Modernisierung von komplexen Softwaresystemen"
permalink: "/modernisierung/"
header:
  -title: "Softwaremodernisierung"
---
<ul>
    {% for post in site.categories.modernisierung %}
    <li><a href="{{ site.url }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
