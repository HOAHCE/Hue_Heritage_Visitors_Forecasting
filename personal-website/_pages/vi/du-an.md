---
layout: page
permalink: /vi/du-an/
title: Dự án
lang: vi
nav: false
description: Mã nguồn, dữ liệu và các sản phẩm ứng dụng.
display_categories: [research, teaching]
---

{% include vi_nav.liquid %}

<div class="projects">
{% for category in page.display_categories %}
  <a id="{{ category }}" href=".#{{ category }}">
    <h2 class="category">{{ category }}</h2>
  </a>
  {% assign categorized_projects = site.projects | where: "category", category %}
  {% assign sorted_projects = categorized_projects | sort: "importance" %}
  <div class="row row-cols-1 row-cols-md-3">
    {% for project in sorted_projects %}
      {% include projects.liquid %}
    {% endfor %}
  </div>
{% endfor %}
</div>
