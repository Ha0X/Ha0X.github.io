---
title: "Others"
layout: single
permalink: /notes/others/
---

{% assign other_posts = site.posts | where_exp: "p", "p.categories contains 'Others'" | sort: 'date' | reverse %}
{% if other_posts.size > 0 %}
<ul style="list-style: none; padding-left: 0;">
  {% for post in other_posts %}
  <li style="margin-bottom: 2rem; padding-bottom: 1.5rem; border-bottom: 1px solid #ecf0f1;">
    <strong style="font-size: 1.25rem; font-weight: 600;"><a href="{{ post.url | relative_url }}" style="color: #2c3e50; text-decoration: none;">{{ post.title }}</a></strong>
    <div style="margin-top: 0.5rem; font-size: 1rem; line-height: 1.6; color: #7f8c8d;">{{ post.excerpt | strip_html | truncate: 180 }}</div>
  </li>
  {% endfor %}
</ul>
{% else %}
<p style="font-size: 1.05rem; color: #7f8c8d;">暂无笔记，稍后更新。</p>
{% endif %}