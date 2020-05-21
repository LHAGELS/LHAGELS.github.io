---
layout: archive
permalink: /blog/
title: Blog
author_profile: true
classes: wide
---

{% for post in site.posts %}
    <hr>
    {% capture day %}{{ post.date | date: '%m%d%Y' }}{% endcapture %}
    {% capture nday %}{{ post.next.date | date: '%m%d%Y' }}{% endcapture %}

    {% if day != nday %}
        <h5 class="date">{{ post.date | date: "%A, %B %e, %Y" }}</h5>
    {% endif %}
    <hr>

{% endfor %}
