---
layout: single
permalink: /blog/
title: Blog Posts
---

Only old things here. See the [STRUDEL blog](https://cmustrudel.github.io/blog/) 
for more recent posts.

{% for post in site.posts %}
  * {{ post.date | date_to_string }} &raquo; [ {{ post.title }} ]({{ post.url }})
{% endfor %}