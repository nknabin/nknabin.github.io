---
layout: page
title: Wander
permalink: /wander/
---

This is what happens when the mind goes for a wander. If you are prepared to go down a stupid lane of silly thoughts and stories, just pick a lane and head down here!

<ul>
  {% for post in site.categories.wander %}
    {% if post.url %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>
