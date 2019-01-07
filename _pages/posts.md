---
layout: archives
permalink: /blog/
title: ""
author_profile: true
header:
  image: '/assets/image/about-color.jpg'
---

{% for post in site.posts %}
  <a href="{{ post.url }}">
    <h2>{{ post.title }}</h2>
    <p>{{ post.date | date_to_string }}</p>
  </a>
{% endfor %}
