---
layout: page
title: Welcome to Voice of Hugo!
---
{% include JB/setup %}
<ul class="posts">
  {% for post in site.posts %}
    <span>{{ post.date | date_to_string }}</span>
    <a href="{{ BASE_PATH }}{{ post.url }}">{{ <h2>post.title</h2> }}</a>
    <div class="content">
      {{ post.content }}
    </div>
  {% endfor %}
</ul>
