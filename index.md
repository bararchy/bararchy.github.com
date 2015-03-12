---
layout: page
title: On Development and Security
---
{% include JB/setup %}




Posts by date:

<ul class="posts">
  {% for post in site.posts %}
    <li><span>{{ post.date | date_to_string }}</span> &raquo; <a href="{{ BASE_PATH }}{{ post.url }}">{{ post.title }}</a></li>
  {% endfor %}
</ul>


    def testing(test)
      puts test
    end

    testing('hi')
    => "hi"
