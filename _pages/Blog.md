---
layout: archive
permalink: /blog/
title: Blog
author_profile: true
classes: wide
---

<ul>
  {% for post in site.posts limit:3 %}
    <li>
      <a href="{{ post.url }} {{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
