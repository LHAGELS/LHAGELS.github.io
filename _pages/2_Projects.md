---
layout: archive
permalink: /projects/
title: "Projects"
author_profile: true
header:
  image: ""
---

{% include base_path %}
{% include group-by-array collection=site.posts field="categories" %}

{% for categories in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ categories | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
