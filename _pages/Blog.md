---
layout: recent-posts
permalink: /recent-posts/
title: Recent posts
author_profile: true
classes: wide
---

<div class="blog-index">  
  {% assign post = site.posts.first %}
  {% assign post = site.posts.second %}
  {% include post_detail.html %}
</div>
