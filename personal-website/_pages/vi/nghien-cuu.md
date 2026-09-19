---
layout: page
permalink: /vi/nghien-cuu/
title: Nghiên cứu khoa học
lang: vi
nav: false
description: Bài báo khoa học, báo cáo hội thảo và đề tài nghiên cứu.
---

{% include vi_nav.liquid %}

Danh mục dưới đây được đồng bộ với hồ sơ tại
[CSDL Khoa học Đại học Huế](https://csdlkhoahoc.hueuni.edu.vn/index.php/scientist/detail/id/2119).

## Công bố khoa học

<div class="publications">

{% bibliography %}

</div>

## Đề tài nghiên cứu

{% assign grants = site.data.grants | sort: "start_year" | reverse %}
{% if grants and grants.size > 0 %}
<ul>
{% for g in grants %}
  <li style="margin-bottom: 0.9rem;">
    <strong>{{ g.title_vi | default: g.title }}</strong><br>
    <span class="text-muted">
      {{ g.level_vi | default: g.level }}{% if g.code %} &middot; {{ g.code }}{% endif %} &middot;
      {{ g.role_vi | default: g.role }} &middot;
      {{ g.start_year }}{% if g.end_year %}&ndash;{{ g.end_year }}{% endif %}
      {% if g.status_vi or g.status %} &middot; {{ g.status_vi | default: g.status }}{% endif %}
    </span>
    {% if g.description_vi or g.description %}<br>{{ g.description_vi | default: g.description }}{% endif %}
  </li>
{% endfor %}
</ul>
{% else %}
<p class="text-muted">Danh sách đề tài được khai báo trong <code>_data/grants.yml</code>.</p>
{% endif %}
