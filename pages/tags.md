---
layout: page
title: Tags
permalink: /tags/
---

{% assign all_tags = site.posts | map: "tags" | join: "," | split: "," | sort | uniq %}
{% for tag in all_tags %}
{% assign tag_stripped = tag | strip %}
{% if tag_stripped != "" %}
<h2 id="{{ tag_stripped | slugify }}">{{ tag_stripped }}</h2>
<ul>
{% for post in site.posts %}
{% if post.tags contains tag_stripped %}
  <li><span class="post-date">{{ post.date | date: "%Y-%m-%d" }}</span> &mdash; <a href="{{ post.url }}">{{ post.title }}</a></li>
{% endif %}
{% endfor %}
</ul>
{% endif %}
{% endfor %}
