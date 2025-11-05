---
title: ""
layout: single
permalink: /
author_profile: false
excerpt: ""
---

<div id="about">
  <h2>👋 简介</h2>
  <p>这里写一段简单的自我介绍。</p>
  <h2>🛠 技能</h2>
  <ul>
    <li>前端开发：HTML / CSS / JavaScript / React</li>
    <li>后端开发：Python / Flask / Node.js</li>
    <li>数据分析：Pandas / NumPy / SQL</li>
    <li>工具：Git / Docker / VS Code</li>
  </ul>
  <h2>💼 经历</h2>
  <ul>
    <li>2023.07 - 至今：某公司 前端开发</li>
    <li>2022.03 - 2023.06：某公司 实习生</li>
  </ul>
  <h2>📫 联系我</h2>
  <ul>
    <li>邮箱：<a href="mailto:youremail@example.com">youremail@example.com</a></li>
    <li>GitHub：<a href="https://github.com/Ha0X">https://github.com/Ha0X</a></li>
    <li>LinkedIn：<a href="https://linkedin.com/in/你的ID">https://linkedin.com/in/你的ID</a></li>
  </ul>
</div>

<div id="projects">
  <h2>我的项目</h2>
  <p>这里是我做过的一些项目：</p>
  {% assign items = site.data.projects.projects %}
  <ul>
    {% for item in items %}
    <li>
      <strong><a href="{{ item.url }}" target="_blank" rel="noopener">{{ item.title }}</a></strong>
      <div>{{ item.excerpt }}</div>
    </li>
    {% endfor %}
  </ul>
</div>
