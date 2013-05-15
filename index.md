---
layout: page
title: Welcome to Voice of Hugo!
---
{% include JB/setup %}
<ul class="posts">
  {% for post in site.posts %}
    <span><h5><b>{{ post.date | date_to_string }}</h5></b></span>
    <br />
    <span><a href="{{ BASE_PATH }}{{ post.url }}"><h3> <b> {{ post.title }} </b></h3></a></span>
    
<div class="post-content-truncate">
  {% if post.content contains "<!-- more -->" %}
    {{ post.content | split:"<!-- more -->" | first % }}
  {% else %}
    {{ post.content | strip_html | truncatewords:100 }}
  {% endif %}
</div>
<br /> <br />
  {% endfor %}
</ul>