---
layout: page
show_meta: false
title: "Softwareentwicklung"
permalink: "/entwicklung/"
header:
  - title: "Softwareentwicklung"
---
<ul>
    {% for post in site.categories.entwicklung %}
    <li><a href="{{ site.url }}{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a></li>
    {% endfor %}
</ul>
