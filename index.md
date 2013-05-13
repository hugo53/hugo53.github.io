---
layout: page
title: Welcome to Voice of Hugo!
---
{% include JB/setup %}
<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.hour | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>
