---
title: ""
layout: single
permalink: /
author_profile: false
excerpt: ""
---

<div id="about">
  <h2>简介</h2>
  <p>我是徐宇昊，复旦大学智能科学与技术专业，目前的研究学习兴趣包括但不限于：</p>
  <ul>
    <li>机器人</li>
    <li>强化学习</li>
    <li>世界模型</li>
  </ul>
  <p>希望能让机器人更加智能，走进人们的生活。</p>
  <h2>技能</h2>
  <ul>
    <li>数学基础：线性代数 / 微积分 / 概率论 / 随机过程</li>
    <li>机器人学：机器人学导论 / ROS2 / 自动控制原理</li>
    <li>编程语言：Python / C++ </li>
    <li>工具：Git / Docker / Linux</li>
  </ul>
  <h2>经历</h2>
  <ul>
    <li>2024.09 - 至今：复旦大学</li>
  </ul>
  <h2>联系我</h2>
  <ul>
    <li>邮箱：<a href="24300830004@m.fudan.edu.cn">24300830004@m.fudan.edu.cn</a></li>
    <li>GitHub：<a href="https://github.com/Ha0X">https://github.com/Ha0X</a></li>
  </ul>
</div>

<div id="projects">
  <h2>我的项目</h2>
  <p>这里是我做过的一些项目：</p>
  {% assign items = site.data.projects.projects %}
  <ul>
    {% for item in items %}
    <li>
      <strong>{{ item.title }}</strong>
      <div>
        {{ item.excerpt }}
        <span> — <a href="{{ item.url }}" target="_blank" rel="noopener">GitHub 仓库</a></span>
      </div>
    </li>
    {% endfor %}
  </ul>
</div>

<div id="notes">
  <h2>学习笔记</h2>
  <p>这里展示部分 Markdown 笔记内容（点击标题查看完整内容）：</p>
  {% assign posts = site.posts | sort: 'date' | reverse %}
  <ul>
    {% for post in posts limit:3 %}
    <li>
      <strong><a href="{{ post.url | relative_url }}">{{ post.title }}</a></strong>
      <div>{{ post.excerpt | strip_html | truncate: 180 }}</div>
    </li>
    {% endfor %}
  </ul>
</div>
