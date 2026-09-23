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

<div class="publications">
{% assign grants_by_year = site.data.grants | group_by: "start_year" | sort: "name" | reverse %}
{% for year_group in grants_by_year %}
  <h2 class="bibliography">{{ year_group.name }}</h2>
  <ul class="grant-list">
  {% for g in year_group.items %}
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
{% endfor %}
</div>
