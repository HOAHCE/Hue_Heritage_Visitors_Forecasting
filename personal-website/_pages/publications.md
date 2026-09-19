---
layout: page
permalink: /publications/
title: research
description: Journal articles, conference papers and funded projects, in reverse chronological order.
nav: true
nav_order: 1
---

<!-- Ô tìm kiếm trong danh mục công bố -->
{% include bib_search.liquid %}

<div class="publications">

{% bibliography %}

</div>

---

## Funded projects &middot; Đề tài nghiên cứu

{% assign grants = site.data.grants | sort: "start_year" | reverse %}
{% if grants and grants.size > 0 %}
<ul>
{% for g in grants %}
  <li style="margin-bottom: 0.9rem;">
    <strong>{{ g.title }}</strong><br>
    <span class="text-muted">
      {{ g.level }}{% if g.code %} &middot; {{ g.code }}{% endif %} &middot;
      {{ g.role }} &middot; {{ g.start_year }}{% if g.end_year %}&ndash;{{ g.end_year }}{% endif %}
      {% if g.status %} &middot; {{ g.status }}{% endif %}
    </span>
    {% if g.description %}<br>{{ g.description }}{% endif %}
    {% if g.url %}<br><a href="{{ g.url }}">Details</a>{% endif %}
  </li>
{% endfor %}
</ul>
{% else %}
<p class="text-muted">Danh sách đề tài sẽ được cập nhật trong <code>_data/grants.yml</code>.</p>
{% endif %}
