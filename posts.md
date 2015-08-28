---
layout: default
permalink: /posts/index.html
---

## Blogbeiträge

<ul>
{% assign posts = site.posts %}
{% for post in posts %}
	<li><a href="{{ site.baseurl }}{{post.url}}">{{post.title}}</a>{% include date.html date=post.date %}</li>
{% endfor %}
</ul>
