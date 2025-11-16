---
layout: page
title: Blog
permalink: /blog/
---

My blog. My notes. My thoughts.

<ul>
  {% for post in site.posts %}
    {% if post.url %}
        <li><span style="color: gray">{{ post.date | date: "%B %d, %Y" }} -- </span><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>
