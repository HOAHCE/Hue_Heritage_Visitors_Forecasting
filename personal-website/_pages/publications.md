---
layout: page
permalink: /publications/
title: Research
lang: en
lang_alt: /vi/nghien-cuu/
nav: true
nav_order: 1
description: Journal articles, conference papers, books and funded projects.
---

Full record on the
[Hue University research database](https://csdlkhoahoc.hueuni.edu.vn/index.php/scientist/detail/id/2119).
Titles of Vietnamese-language publications are kept in the original language.

## Articles and conference papers

<div class="publications">
{% bibliography %}
</div>

## Books and textbooks

<div class="publications">
{% bibliography -f books %}
</div>

## Funded projects

<div class="publications">
{% assign grants_by_year = site.data.grants | group_by: "start_year" | sort: "name" | reverse %}
{% for year_group in grants_by_year %}
  <h2 class="bibliography">{{ year_group.name }}</h2>
  <ul class="grant-list">
  {% for g in year_group.items %}
    <li>
      <strong>{{ g.title }}</strong><br>
      <span class="text-muted">
        {{ g.role }} &middot;
        {{ g.start_year }}{% if g.end_year and g.end_year != g.start_year %}&ndash;{{ g.end_year }}{% endif %}
        {% if g.code %} &middot; {{ g.code }}{% endif %}
      </span>
    </li>
  {% endfor %}
  </ul>
{% endfor %}
</div>
