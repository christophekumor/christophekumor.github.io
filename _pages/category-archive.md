---
layout: archive
permalink: /categories/
title: "Posts by Category"
author_profile: false
---

{% include group-by-array collection=site.posts field="categories" %}

{% for category in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ category | slugify }}" class="archive__subtitle">{{ category }}</h2>
  {% for post in posts %}
     {% if post.tags contains 'draft' %}
  {% else %} 
    {% include archive-single.html %}
  {% endif %}
  {% endfor %}
{% endfor %}