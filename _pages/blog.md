---
title:  "Blog"
layout: archive
permalink: /blog/
author_profile: true
comments: false
---

<ul>
  {% for post in site.posts %}
    <li>
      <a href="{{ post.url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
