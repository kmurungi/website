---
layout: about
title: About
permalink: /
subtitle: Master's student in Computer Vision (MSCV) · Robotics Institute · Carnegie Mellon University

profile:
  align: right
  # image: kmurungi.jpeg
  image_circular: True # crops the image to make it circular
  more_info: >

selected_papers: false # includes a list of papers marked as "selected={true}"
social: true # includes social icons at the bottom of the page

announcements:
  enabled: false # includes a list of news items
  scrollable: true # adds a vertical scroll bar if there are more than 3 news items
  limit: 5 # leave blank to include all the news in the `_news` folder

latest_posts:
  enabled: false
  scrollable: false # adds a vertical scroll bar if there are more than 3 new posts items
  limit: 3 # leave blank to include all the blog posts
---

I’m a Master’s student in Computer Vision (MSCV) at Carnegie Mellon University’s Robotics Institute. My interests are spatial AI and 3D scene understanding: how machines perceive, index, and reason about physical environments.

Below are recent projects on spatial scene understanding—native 3D representations, open-vocabulary retrieval, and grounding language models on 3D evidence.


<div class="mt-4" style="display:flex;flex-direction:column;gap:1.25rem;">
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
