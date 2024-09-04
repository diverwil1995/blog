---
layout: page
title: Lists
---

{% for list in site.lists %}
  <h2><a href="{{ list.url | relative_url }}">{{ list.title }}</a></h2>
  <p>{{ list.description }}</p>
{% endfor %}