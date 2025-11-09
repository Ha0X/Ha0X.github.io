---
title: "运动控制 主题"
layout: single
permalink: /notes/motion/
---

<h2>运动控制主题笔记</h2>
<p>这里汇总所有标注为 <code>运动控制</code> 的学习笔记：</p>

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