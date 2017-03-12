---
layout: page
title: Welcome to my blog!
tagline: Supporting tagline
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
  	<li>
      <a href="{{ BASE_PATH }}{{ post.url }}"><h4>{{ post.title }}</h4></a>
      <span>{{ post.date | date_to_string }}</span> 
      {{ post.excerpt }}
    </li>
<!--     <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li> -->
  {% endfor %}
</ul>
