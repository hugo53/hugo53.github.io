---
layout: page
title: Welcome to Voice of Hugo!
---
{% include JB/setup %}
<ul class="posts">
  {% for post in site.posts %}
    <span><b>{{ post.date | date_to_string }}</b></span>
    <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a>
    <br />
    <div class="content">
      {{ post.content }}
    </div>
  {% endfor %}
</ul>
