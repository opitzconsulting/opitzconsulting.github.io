---
layout: default
permalink: /posts/index.html
---

<h3>Blogbeiträge</h3>

<ul>
{% assign posts = site.posts %}
{% for post in posts %}
	<li><a href="{{post.url}}">{{post.title}}</a>{% include date.html date=post.date %}</li>
{% endfor %}
</ul>
