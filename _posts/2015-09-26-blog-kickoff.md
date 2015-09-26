--- 
layout: page
header: no
title: "OC goes JekyllBlog"
teaser: "Wie kann man einen Jekyll basierten Blog nutzen?"
author: Pascal Brokmeier und Frank Sanders
categories:
- Automatisierung
- SocialBlogging
- 
---

Während der MSA Days 2015 haben Stefan Glase und Richard Attermeyer eine Initiative vorgestellt, den Mitarbeitern mittels Github & Jekyll eine Möglichkeit zu bieten, Blogposts zu erstellen. Um allen einen einfachen Einstieg zu ermöglichen und direkt auch Content zu erhalten, sollte jeder Zuhörer einen eigenen Artikel verfassen und mittels Pull Request auf Github zum Blog hinzufügen. Dieser Artikel beschreibt, wie dieser Prozess verlaufen ist.

![]({{ site.baseurl }}/assets/img/2015-09-26-blog-kickoff/oc-goes-jekyll-1.jpg)

Zuerst wurden die Technologien kurz vorgestellt. Hierfür wurde eine schnelle Einführung in Jekyll, sowie Github Forks und Pull Requests gegeben. So hatte jeder das notwendige Wissen um seinen ersten eigenen Blogpost zu erstellen. Da Posts mittels Markdown erstellt werden, ist das erstellen eines Posts selbst für nicht-Softwareentwickler einfach möglich, obwohl kein `WYSIWYG` Konzept verfolgt wird. 

Direkt nach der Einführung wurden die ersten Posts (diesen eingeschlossen) erstellt. Die Posts, welche erstellt werden sollten sind:

- OC goes JekyllBlog
- Cattlecrew Artikel
- SD Days 2015
- JDBC REST Streaming
- SQL String in mehrere Zeilen aufteilen (oder so, nur länger)

Ob diese es bis heute Abend in Github und auf den Blog schaffen, wird sich herausstellen. Alle Teilnehmer waren jedenfalls sehr begeistert von der einfachen Erstellung von Posts mit ihnen bekannten mitteln. Entwickler können das sowieso oft verwendete Versionierunsgsystem einsetzen um einen Post zu schreiben. Wenn der jeweilige Entwickler Schreibrechte auf das eigentliche Repository besitzt, sogar komplett ohne den Einsatz des Browsers, für besonders hart gesonnene nur mittels der Kommandozeile, `vi & git`. 

Um einen Post zu erstellen müssen folgende Schritte durchgeführt werden:

1. Fork des Repositories, welches den Blog enthält ([Dieser Blog](https://github.com/opitzconsulting/blog.git))
2. Lokalen Klonen mit `git clone URL_TO_YOUR_FORK`
3. Lokal einen Branch erstellen mit `git checkout -b newPostName`
4. Post verfassen und als Datei unter `_posts` ablegen. Die Struktur der bereits bestehenden Posts kann als Vorlage genommen werden. Der Name der Datei muss dem Format `yyyy-mm-dd-newPostName.md` entsprechen
	* Ggf. Bilder unter `assets/img/yyyy-mm-dd-newPostName/` ablegen.
5. Die geänderten Dateien committen & pushen 
6. Einen Pull Request erstellen

Abschließend noch ein paar Impressionen:

![]({{ site.baseurl }}/assets/img/2015-09-26-blog-kickoff/oc-goes-jekyll-2.jpg)

<br/>

![]({{ site.baseurl }}/assets/img/2015-09-26-blog-kickoff/oc-goes-jekyll-3.jpg)



