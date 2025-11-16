---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: home
---

I am a technology enthusiast and find computers to be like magic. I have always enjoyed playing around on a computer, no matter what I did. From playing games to writing my own software, everything that I did has been a lot of fun. I have learned a lot and spilled pieces of my knowledge here, as a means to help myself when I am in need but also others who are on the same path.

This website contains technical guides and information on various topics related to software development, Linux administration, web security and more. I try to keep all the information up to date, making necessary changes when things need to be updated.

There are some silly stuff on the blog as well. Give it a try and prepare to be bored.

With the intention of helping myself when I come here after a few years, I have tried to be as beginner friendly as possible in my articles. I wish you all good luck and enjoy your time here.

Here are my recent posts! You can find more in the <a href="/blog">Blog</a> section.

<ul>
  {% for post in site.posts limit:3 %}
    {% if post.url %}
        <li><span style="color: gray">{{ post.date | date: "%B %d, %Y" }} </span><a href="{{ post.url }}">{{ post.title }}</a></li>
        <p>{{ post.excerpt | strip_html | truncate: 160 }}</p>
    {% endif %}
  {% endfor %}
</ul>

