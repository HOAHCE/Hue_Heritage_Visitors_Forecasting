---
layout: page
permalink: /vi/nghien-cuu/
title: Nghiên cứu khoa học
nav_title: Nghiên cứu
lang: vi
lang_alt: /publications/
nav: true
nav_order: 1
description: Bài báo, báo cáo hội thảo, sách, giáo trình và đề tài nghiên cứu.
---

Danh mục dưới đây được đồng bộ với hồ sơ tại
[CSDL Khoa học Đại học Huế](https://csdlkhoahoc.hueuni.edu.vn/index.php/scientist/detail/id/2119).

## Bài báo và báo cáo khoa học

<div class="publications">
{% bibliography %}
</div>

## Sách, giáo trình

<div class="publications">
{% bibliography -f books %}
</div>

## Đề tài nghiên cứu

{% assign grants = site.data.grants | sort: "start_year" | reverse %}
<ul class="grant-list">
{% for g in grants %}
  <li>
    <strong>{{ g.title_vi | default: g.title }}</strong><br>
    <span class="text-muted">
      {{ g.role_vi | default: g.role }} &middot;
      {{ g.start_year }}{% if g.end_year and g.end_year != g.start_year %}&ndash;{{ g.end_year }}{% endif %}
      {% if g.code %} &middot; Mã số: {{ g.code }}{% endif %}
      {% if g.pi_vi %}<br>Chủ nhiệm: {{ g.pi_vi }}{% endif %}
      {% if g.members_vi %}<br>Thành viên: {{ g.members_vi }}{% endif %}
    </span>
  </li>
{% endfor %}
</ul>
