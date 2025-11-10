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
  <p>希望能让机器人更加智能</p>
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
  <h2>项目</h2>
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
  <p>按主题浏览：</p>
  <ul>
    <li><a href="{{ "/notes/vla/" | relative_url }}">VLA</a></li>
    <li><a href="{{ "/notes/world-model/" | relative_url }}">World Model</a></li>
    <li><a href="{{ "/notes/dl/" | relative_url }}">深度学习</a></li>
    <li><a href="{{ "/notes/rl/" | relative_url }}">强化学习</a></li>
    <li><a href="{{ "/notes/motion/" | relative_url }}">运动控制</a></li>
    <li><a href="{{ "/notes/others/" | relative_url }}">Others</a></li>
    <li><a href="{{ "/notes/new/" | relative_url }}">新建</a></li>
  </ul>
</div>
