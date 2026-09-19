---
layout: page
permalink: /teaching/
title: teaching
description: Courses I teach, with syllabi, lecture slides and lab materials.
nav: true
nav_order: 2
---

Tài liệu dưới đây dành cho sinh viên các học phần tôi phụ trách. Mọi góp ý xin gửi qua email.

{% include courses.liquid %}

---

## Tài liệu chung &middot; General resources

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
