---
layout: page
title: On Development and Security
---
{% include JB/setup %}
<link rel="stylesheet" type="text/css" href="assets/themes/bootstrap-3/css/style.css">
<a href="{{ BASE_PATH }}/categories.html"> All Categories </a>

Posts by date:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>




