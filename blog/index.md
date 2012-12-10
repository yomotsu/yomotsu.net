---
layout: index
title: ヨモツネット
---

<div class="mod-articleList">{% for post in site.posts %}
	<a href="{{ site.baseurl }}{{ post.url }}"><article>
		<h1>{{ post.title }}</h1>
		<div>Posted : <time pubdate="pubdate" datetime="{{ post.date | date_to_xmlschema }}">{{ post.date | date: '%b' }} {{ post.date | date: '%d' }}, {{ post.date | date: '%Y' }}</time></div>
	</article></a>
{% endfor %}</div>