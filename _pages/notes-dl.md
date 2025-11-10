---
title: "深度学习"
layout: single
permalink: /notes/dl/
---


{% assign dl_posts = site.posts | where_exp: "p", "p.categories contains '深度学习'" | sort: 'date' | reverse %}
{% if dl_posts.size > 0 %}
<ul>
  {% for post in dl_posts %}
  <li>
    <strong><a href="{{ post.url | relative_url }}">{{ post.title }}</a></strong>
    <div>{{ post.excerpt | strip_html | truncate: 180 }}</div>
  </li>
  {% endfor %}
</ul>
{% else %}
<p>暂无笔记，稍后更新。</p>
{% endif %}