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

{% include bib_search.liquid %}

<div class="publications">

{% bibliography %}

</div>

## Funded projects

{% assign grants = site.data.grants | sort: "start_year" | reverse %}
<ul>
{% for g in grants %}
  <li style="margin-bottom: 0.9rem;">
    <strong>{{ g.title }}</strong><br>
    <span class="text-muted">
      {{ g.level }}{% if g.code and g.code != "" %} &middot; {{ g.code }}{% endif %} &middot;
      {{ g.role }} &middot; {{ g.start_year }}{% if g.end_year %}&ndash;{{ g.end_year }}{% endif %}
      {% if g.status %} &middot; {{ g.status }}{% endif %}
    </span>
    {% if g.description %}<br>{{ g.description }}{% endif %}
  </li>
{% endfor %}
</ul>

<p class="text-muted">
  Full record on the
  <a href="https://csdlkhoahoc.hueuni.edu.vn/index.php/scientist/detail/id/2119">Hue University research database</a>.
  Titles of Vietnamese-language publications are kept in the original language.
</p>
