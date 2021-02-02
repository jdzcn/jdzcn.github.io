---
layout: default
title: 生活
permalink: /life/
---

<ul>
  {% for post in site.posts %}
	{% if post.tags == 'life' %}
		<li>
		      <a href="{{ post.url }}">{{ post.title }}</a>
		      <p>{{ post.excerpt }}</p>
		</li>
	{% endif %} 
  {% endfor %}
</ul>
