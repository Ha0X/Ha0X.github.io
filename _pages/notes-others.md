---
title: "Others 主题"
layout: single
permalink: /notes/others/
---

<h2>Others 主题笔记</h2>
<p>这里汇总所有标注为 <code>Others</code> 的学习笔记：</p>

{% assign other_posts = site.posts | where_exp: "p", "p.categories contains 'Others'" | sort: 'date' | reverse %}
{% if other_posts.size > 0 %}
<ul>
  {% for post in other_posts %}
  <li>
    <strong><a href="{{ post.url | relative_url }}">{{ post.title }}</a></strong>
    <div>{{ post.excerpt | strip_html | truncate: 180 }}</div>
  </li>
  {% endfor %}
</ul>
{% else %}
<p>暂无笔记，稍后更新。</p>
{% endif %}