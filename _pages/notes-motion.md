---
title: "运动控制"
layout: single
permalink: /notes/motion/
---

{% assign motion_posts = site.posts | where_exp: "p", "p.categories contains '运动控制'" | sort: 'date' | reverse %}
{% if motion_posts.size > 0 %}
<ul>
  {% for post in motion_posts %}
  <li>
    <strong><a href="{{ post.url | relative_url }}">{{ post.title }}</a></strong>
    <div>{{ post.excerpt | strip_html | truncate: 180 }}</div>
  </li>
  {% endfor %}
</ul>
{% else %}
<p>暂无笔记，稍后更新。</p>
{% endif %}