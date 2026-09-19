---
layout: page
permalink: /vi/nghien-cuu/
title: Nghiên cứu khoa học
nav_title: Nghiên cứu
lang: vi
lang_alt: /publications/
nav: true
nav_order: 1
description: Bài báo, báo cáo hội thảo, sách và đề tài nghiên cứu.
---

<div class="publications">

{% bibliography %}

</div>

## Đề tài nghiên cứu

{% assign grants = site.data.grants | sort: "start_year" | reverse %}
<ul>
{% for g in grants %}
  <li style="margin-bottom: 0.9rem;">
    <strong>{{ g.title_vi | default: g.title }}</strong><br>
    <span class="text-muted">
      {{ g.level_vi | default: g.level }}{% if g.code and g.code != "" %} &middot; {{ g.code }}{% endif %} &middot;
      {{ g.role_vi | default: g.role }} &middot;
      {{ g.start_year }}{% if g.end_year %}&ndash;{{ g.end_year }}{% endif %}
      {% if g.status_vi or g.status %} &middot; {{ g.status_vi | default: g.status }}{% endif %}
    </span>
    {% if g.description_vi or g.description %}<br>{{ g.description_vi | default: g.description }}{% endif %}
  </li>
{% endfor %}
</ul>

<p class="text-muted">
  Hồ sơ đầy đủ tại
  <a href="https://csdlkhoahoc.hueuni.edu.vn/index.php/scientist/detail/id/2119">CSDL Khoa học Đại học Huế</a>.
</p>
