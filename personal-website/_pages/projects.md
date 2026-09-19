---
layout: page
permalink: /projects/
title: Projects
lang: en
lang_alt: /vi/du-an/
nav: true
nav_order: 3
description: Code, data and applied work.
---

<div class="projects">
  <div class="row row-cols-1 row-cols-md-3">
  {% assign sorted_projects = site.projects | sort: "importance" %}
  {% for project in sorted_projects %}
    {% include projects.liquid %}
  {% endfor %}
  </div>
</div>
