---
title: "World Model"
layout: single
permalink: /notes/world-model/
---


{% assign wm_posts = site.posts | where_exp: "p", "p.categories contains 'World Model'" | sort: 'date' | reverse %}
{% if wm_posts.size > 0 %}
<ul>
  {% for post in wm_posts %}
  <li>
    <strong><a href="{{ post.url | relative_url }}">{{ post.title }}</a></strong>
    <div>{{ post.excerpt | strip_html | truncate: 180 }}</div>
  </li>
  {% endfor %}
</ul>
{% else %}
<p>暂无笔记，稍后更新。</p>
{% endif %}