---
layout: archive
permalink: /home/
author_profile: true
title: "Hallo"
header:
    overlay_image: /assets/images/Header1.jpg
    caption: "Photo by ...  on [Link](...)"
classes: wide
---

{% include base_path %}
{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}
