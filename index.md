---
layout: default
title: my blog index
---

<h2>{{ page.title }}</h2>

<p>最新文章</p>
<ul>
	{% for post in site.posts %}
		<li>
			{{post.data | data_to_string}}
			<a href="{{ site.baseurl }}{{ post.url }}">{{ post.title }}</a>
		</li>
	{% endfor%}
</ul>