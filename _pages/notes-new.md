---
title: "未分类"
layout: single
permalink: /notes/new/
---

{% assign new_posts = site.posts | where_exp: "p", "p.categories contains '新建'" | sort: 'date' | reverse %}
{% if new_posts.size > 0 %}
<ul>
  {% for post in new_posts %}
  <li>
    <strong><a href="{{ post.url | relative_url }}">{{ post.title }}</a></strong>
    <div>{{ post.excerpt | strip_html | truncate: 180 }}</div>
  </li>
  {% endfor %}
</ul>
{% else %}
<p>暂无笔记，稍后更新。</p>
{% endif %}