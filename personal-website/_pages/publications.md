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

{% include bib_search.liquid %}

<div class="publications">
{% bibliography %}
</div>

## Books and textbooks

<div class="publications">
{% bibliography -f books %}
</div>

## Funded projects

{% assign grants = site.data.grants | sort: "start_year" | reverse %}
<ul class="grant-list">
{% for g in grants %}
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
