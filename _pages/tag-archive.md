---
layout: tags
permalink: /tags/
title: "Posts by Tags"
author_profile: true
header:
  overlay_image: http://parallelpath.com/blog/wp-content/uploads/2013/08/tags.jpg
#  overlay_color: rgba(51, 51, 51, 1.0)
  overlay_filter: rgba(51, 51, 51, 0.8)
---

{% include group-by-array collection=site.posts field="tags" %}

{% for tag in group_names %}
  {% assign posts = group_items[forloop.index0] %}
  <h2 id="{{ tag | slugify }}" class="archive__subtitle">{{ tag }}</h2>
  {% for post in posts %}
    {% include archive-single.html %}
  {% endfor %}
{% endfor %}