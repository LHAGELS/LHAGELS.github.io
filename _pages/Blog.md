---
layout: archive
permalink: /blog/
title: Blog
author_profile: true
classes: wide
---


{% include group-by-array collection=site.posts field="tags" %}


{% assign posts = group_items %}
<h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
{% for post in posts %}
  {% include archive-single.html %}
{% endfor %}
