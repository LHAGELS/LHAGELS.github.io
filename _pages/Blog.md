---
layout: archive
permalink: /blog/
title: Blog
author_profile: true
classes: wide
---

{% assign thedate = '' %}

{% for post in site.posts %}

    {% if thedate != post.date | date: "%m-%d-%Y" %}
        <h2>{{ post.date | date: "%A, %B %e, %Y" }}</h2>
    {% endif %}

    {% assign thedate = post.date | date: "%m-%d-%Y" %}

    <h3 class="headline"><a href="{{ post.url }}">{{ post.title }}</a></h3>
    {{ post.content }}
    <hr>

{% endfor %}
