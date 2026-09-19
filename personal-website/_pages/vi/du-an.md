---
layout: page
permalink: /vi/du-an/
title: Dự án
lang: vi
lang_alt: /projects/
nav: true
nav_order: 3
description: Mã nguồn, dữ liệu và các sản phẩm ứng dụng.
---

<div class="projects">
  <div class="row row-cols-1 row-cols-md-3">
  {% assign sorted_projects = site.projects | sort: "importance" %}
  {% for project in sorted_projects %}
    {% include projects.liquid %}
  {% endfor %}
  </div>
</div>
