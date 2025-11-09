---
title: "新建 主题"
layout: single
permalink: /notes/new/
---

<h2>新建 主题笔记</h2>
<p>这里汇总所有标注为 <code>新建</code> 的学习笔记：</p>

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