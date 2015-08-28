---
layout: default
---

## Herzlich Willkommen

Dieser Blog ...

## Neueste Beiträge

{% for post in site.posts limit:5 %}
### <a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
<small>{% include date.html date=post.date %}</small>
<p>{{ post.excerpt | strip_html }} <a href="{{ site.baseurl }}{{ post.url }}">Weiterlesen</a></p>
{% endfor %}

Finden Sie hier eine Liste <a href="{{ site.baseurl }}/posts">aller Blogbeiträge</a>.