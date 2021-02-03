---
layout: default
title: Other
permalink: /other/
---

<ul>
  {% for post in site.posts %}
	{% if post.tags contains 'other' %}
		<li>
		      <a href="{{ post.url }}">{{ post.title }}</a>
		      
		</li>
	{% endif %} 
  {% endfor %}
</ul>
