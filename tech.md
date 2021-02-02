---
layout: default
title: 技术
permalink: /tech/
---

<ul>
  {% for post in site.posts %}
	{% if page.tags == 'tech' %}
		<li>
		      <a href="{{ post.url }}">{{ post.title }}</a>
		      <p>{{ post.excerpt }}</p>
		</li>
	{% endif %} 
  {% endfor %}
</ul>
