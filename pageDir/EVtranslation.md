---
layout: page
title: "EVtranslation"
---
{% include JB/setup %}
> This page is for saving some translation posts which I took time to translate them from English into Vietnamese. What I want to say is that I have no permission to own all of original articles. My work is just for improve translation skill, not for any commercial purposes. 

<ul class="posts">
  {% for post in site.posts %}
 	{% if post.category == 'translation' %}
    	<span><h5><b>{{ post.date | date_to_string }}</b></h5></span>
    	<span><a href="{{ BASE_PATH }}{{ post.url }}"><h3> <b> {{ post.title }} </b></h3></a></span>
   		<div class="post-content-truncate">
  			{% if post.description != empty %}
  	  		{{ post.description }}
  			{% else %}
    			{{ post.content | strip_html | truncatewords:100 }}
  			{% endif %}
		</div>
		<br /> <br />
    {% endif %}
  {% endfor %}
</ul>