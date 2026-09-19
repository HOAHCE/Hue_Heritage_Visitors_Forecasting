---
layout: page
permalink: /vi/giang-day/
title: Giảng dạy
lang: vi
nav: false
description: Học phần phụ trách, đề cương, bài giảng và tài liệu thực hành.
---

{% include vi_nav.liquid %}

Tài liệu dưới đây dành cho sinh viên các học phần tôi phụ trách.
Mọi góp ý hoặc đề nghị bổ sung tài liệu, sinh viên vui lòng gửi qua email.

{% include courses.liquid %}

## Tài liệu chung

{% assign res = site.data.resources %}
{% if res and res.size > 0 %}
<ul>
{% for r in res %}
  <li><a href="{{ r.url | relative_url }}">{{ r.name }}</a>{% if r.description %} — {{ r.description }}{% endif %}</li>
{% endfor %}
</ul>
{% else %}
<p class="text-muted">Danh sách tài liệu chung được khai báo trong <code>_data/resources.yml</code>.</p>
{% endif %}
