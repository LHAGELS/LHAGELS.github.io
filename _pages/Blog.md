---
layout: single
permalink: /recent-posts/
title: Recent posts
author_profile: true
classes: wide
---

<div class="blog-index">  
  {% assign post = site.posts.first %}
  {% assign content = post.content %}
  {% include post_detail.html %}
</div>
