---
layout: page
title: Welcome to my blog!
tagline: Supporting tagline
---
{% include JB/setup %}

<ul class="posts">
  {% for post in site.posts %}
  	<li>
      <a href="{{ BASE_PATH }}{{ post.url }}"><h4>{{ post.titlePL }}</h4></a>
      <span>{{ post.date | date_to_string }} </span>&middot;{% include read_time.html %}
      {{ post.excerpt }}
	  	{% if post.excerpt != post.content %}
			<p class="ReadMore"><a href="{{ site.baseurl }}{{ post.url }}" >Czytaj dalej <i class="fa fa-arrow-circle-right" aria-hidden="true"></i></a></p>
		{% endif %}
    </li>
<!--     <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li> -->
  {% endfor %}
</ul>