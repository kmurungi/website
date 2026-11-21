---
layout: page
title: projects
permalink: /projects/
description: Systems and research builds spanning 3D perception, CUDA, and spatial AI.
nav: true
nav_order: 2
---

<div style="display:flex;flex-direction:column;gap:1.25rem;">
{% assign sorted_projects = site.projects | sort: "importance" %}
{% for project in sorted_projects %}
  <div>
    <a href="{% if project.redirect %}{{ project.redirect }}{% else %}{{ project.url | relative_url }}{% endif %}" style="text-decoration:underline;text-underline-offset:0.2em;font-weight:600;">{{ project.title }}</a>
    {% if project.description %}
      <div class="text-muted" style="margin-top:0.25rem;line-height:1.45;">{{ project.description }}</div>
    {% endif %}
  </div>
{% endfor %}
</div>
