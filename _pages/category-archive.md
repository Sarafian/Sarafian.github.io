---
layout: categories
permalink: /categories/
title: "Posts by Category"
author_profile: true
header:
  overlay_image: http://www.popularcategories.com/images/categories.jpg
#  overlay_color: rgba(51, 51, 51, 1.0)
  overlay_filter: rgba(51, 51, 51, 0.8)
---

{% include group-by-array collection=site.posts field="categories" %}

{% for category in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ category | slugify }}" class="archive__subtitle">{{ category }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}