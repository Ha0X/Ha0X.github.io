---
title: "强化学习 主题"
layout: single
permalink: /notes/rl/
---

<h2>强化学习主题笔记</h2>
<p>这里汇总所有标注为 <code>强化学习</code> 的学习笔记：</p>

{% assign rl_posts = site.posts | where_exp: "p", "p.categories contains '强化学习'" | sort: 'date' | reverse %}
{% if rl_posts.size > 0 %}
<ul>
  {% for post in rl_posts %}
  <li>
    <strong><a href="{{ post.url | relative_url }}">{{ post.title }}</a></strong>
    <div>{{ post.excerpt | strip_html | truncate: 180 }}</div>
  </li>
  {% endfor %}
</ul>
{% else %}
<p>暂无笔记，稍后更新。</p>
{% endif %}