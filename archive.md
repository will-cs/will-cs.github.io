---
layout: page
title: Archive
permalink: /archive/
---

{% Blog posts %}
{% for syntax is from the Liquid templating system. This is a comment in Liquid as well %}

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}
