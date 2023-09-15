---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---
Welcome to my blog!

You can find my notes here that I have maintained over the years! Just find something that interests you below and click on it.

General
<ul>
  {% for post in site.categories.general %}
    {% if post.url %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>

Git
<ul>
  {% for post in site.categories.git %}
    {% if post.url %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>

Linux
<ul>
  {% for post in site.categories.linux %}
    {% if post.url %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>

Security
<ul>
  {% for post in site.categories.security %}
    {% if post.url %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>

Virtualization
<ul>
  {% for post in site.categories.virtualization %}
    {% if post.url %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>

Wander
<ul>
  {% for post in site.categories.wander %}
    {% if post.url %}
        <li><a href="{{ post.url }}">{{ post.title }}</a></li>
    {% endif %}
  {% endfor %}
</ul>

<script src="https://chat.preprod.rippey.ai/rippey.js"></script>

