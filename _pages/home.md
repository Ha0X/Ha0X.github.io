---
title: ""
layout: single
permalink: /
author_profile: false
excerpt: ""
---

<div id="about" style="margin-bottom: 3rem; max-width: 100%; width: 100%;">
  <h2 style="font-size: 1.6rem; font-weight: 600; margin-bottom: 1rem; color: #2c3e50;">About Me</h2>
  <p style="font-size: 0.95rem; line-height: 1.7; margin-bottom: 1rem; color: #34495e;">I am Yuhao Xu, an undergraduate student majoring in Intelligent Science and Technology at Fudan University. My research interests focus on:</p>
  <ul style="font-size: 0.95rem; line-height: 1.8; margin-bottom: 1.5rem; color: #34495e;">
    <li><strong>Robotics</strong>: Vision-Language-Action (VLA) models, robot control and planning</li>
    <li><strong>Reinforcement Learning</strong>: Deep reinforcement learning algorithms and applications</li>
    <li><strong>World Models</strong>: Model-based learning and prediction</li>
  </ul>
  <p style="font-size: 0.95rem; line-height: 1.7; margin-bottom: 2rem; color: #34495e;">My goal is to explore how to make robots more intelligent and flexible in understanding and executing complex tasks.</p>
  
  <h2 style="font-size: 1.6rem; font-weight: 600; margin-bottom: 1rem; margin-top: 2rem; color: #2c3e50;">Skills</h2>
  <ul style="font-size: 0.95rem; line-height: 1.8; margin-bottom: 1.5rem; color: #34495e;">
    <li><strong>Mathematics</strong>: Linear Algebra, Calculus, Probability Theory, Stochastic Processes</li>
    <li><strong>Robotics</strong>: Introduction to Robotics, ROS2, Automatic Control Theory</li>
    <li><strong>Programming Languages</strong>: Python, C++</li>
    <li><strong>Development Tools</strong>: Git, Docker, Linux</li>
  </ul>
  
  <h2 style="font-size: 1.6rem; font-weight: 600; margin-bottom: 1rem; margin-top: 2rem; color: #2c3e50;">Education</h2>
  <ul style="font-size: 0.95rem; line-height: 1.8; margin-bottom: 1.5rem; color: #34495e;">
    <li><strong>Sep 2024 - Present</strong>: Fudan University, B.S. in Intelligent Science and Technology</li>
  </ul>
  
  <h2 style="font-size: 1.6rem; font-weight: 600; margin-bottom: 1rem; margin-top: 2rem; color: #2c3e50;">Contact</h2>
  <ul style="font-size: 0.95rem; line-height: 1.8; margin-bottom: 1.5rem; color: #34495e;">
    <li><strong>Email</strong>: <a href="mailto:24300830004@m.fudan.edu.cn" style="color: #3498db; text-decoration: none;">24300830004@m.fudan.edu.cn</a></li>
    <li><strong>GitHub</strong>: <a href="https://github.com/Ha0X" target="_blank" rel="noopener" style="color: #3498db; text-decoration: none;">https://github.com/Ha0X</a></li>
  </ul>
</div>

<div id="projects" style="margin-bottom: 3rem; max-width: 100%; width: 100%;">
  <h2 style="font-size: 1.6rem; font-weight: 600; margin-bottom: 1rem; color: #2c3e50;">Projects</h2>
  <p style="font-size: 0.95rem; line-height: 1.7; margin-bottom: 1rem; color: #34495e;">Some projects I have worked on:</p>
  {% assign items = site.data.projects.projects %}
  {% if items.size > 0 %}
  <ul style="font-size: 0.95rem; line-height: 1.8; color: #34495e;">
    {% for item in items %}
    <li style="margin-bottom: 1rem;">
      <strong style="color: #2c3e50;">{{ item.title }}</strong>
      <div style="margin-top: 0.5rem;">
        {{ item.excerpt }}
        <span> — <a href="{{ item.url }}" target="_blank" rel="noopener" style="color: #3498db; text-decoration: none;">GitHub Repository</a></span>
      </div>
    </li>
    {% endfor %}
  </ul>
  {% else %}
  <p style="font-size: 0.95rem; line-height: 1.7; color: #7f8c8d;">Projects information is being updated...</p>
  {% endif %}
</div>

<div id="notes" style="max-width: 100%; width: 100%;">
  <h2 style="font-size: 1.6rem; font-weight: 600; margin-bottom: 1rem; color: #2c3e50;">Research Notes</h2>
  <p style="font-size: 0.95rem; line-height: 1.7; margin-bottom: 1rem; color: #34495e;">Research notes and summaries organized by topic:</p>
  <ul style="font-size: 0.95rem; line-height: 1.8; color: #34495e;">
    <li><a href="{{ "/notes/vla/" | relative_url }}" style="color: #3498db; text-decoration: none;">VLA（视觉-语言-动作）</a></li>
    <li><a href="{{ "/notes/world-model/" | relative_url }}" style="color: #3498db; text-decoration: none;">World Model（世界模型）</a></li>
    <li><a href="{{ "/notes/dl/" | relative_url }}" style="color: #3498db; text-decoration: none;">深度学习</a></li>
    <li><a href="{{ "/notes/rl/" | relative_url }}" style="color: #3498db; text-decoration: none;">强化学习</a></li>
    <li><a href="{{ "/notes/motion/" | relative_url }}" style="color: #3498db; text-decoration: none;">运动控制</a></li>
    <li><a href="{{ "/notes/others/" | relative_url }}" style="color: #3498db; text-decoration: none;">其他主题</a></li>
  </ul>
</div>
