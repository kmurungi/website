---
layout: page
title: Projects
permalink: /projects/
description: Spatial AI and 3D scene understanding — native representations, open-vocabulary retrieval, and language grounded on 3D evidence.
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
