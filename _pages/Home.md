---
layout: archive
permalink: /home/
title: Home
author_profile: true
header:
    overlay_image: /assets/images/Header1.jpg
    caption: "Photo by ...  on [Link](...)"
classes: wide
---
<h3 class="archive__subtitle">{{ site.data.ui-text[site.locale].recent_posts | default: "Recent Posts" }}</h3>

{% for post in paginator.posts %}
  {% include archive-single.html %}
{% endfor %}

{% for post in posts %}
  {% include archive-single.html %}
{% endfor %}

{% include paginator.html %}
