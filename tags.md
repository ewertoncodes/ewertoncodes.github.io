---
layout: page
title: Tags
permalink: /tags/
---

{% assign sorted_tags = site.tags | sort %}

<p>
{% for tag in sorted_tags %}
  {% assign tag_name = tag[0] %}
  {% assign tag_posts = tag[1] %}
  <a href="{{ '/tags/' | append: tag_name | append: '/' | relative_url }}" style="margin-right: 8px; text-decoration: underline;">
    {{ tag_name }} ({{ tag_posts.size }})
  </a>
{% endfor %}
</p>
