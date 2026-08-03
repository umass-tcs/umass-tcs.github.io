---
layout: page
title: "Problem Sets (PSETS)"
permalink: /psets/
---

A central place for problem sets. Maintain entries in <code>_data/psets.yml</code>.

## Problem sets
{% if site.data.psets and site.data.psets.size > 0 %}
<ul class="pset-list">
  {% for ps in site.data.psets %}
  <li>
    <strong>{{ ps.title }}</strong> — {{ ps.course | default: "" }} <span class="muted">({{ ps.term | default: "" }})</span>
    {% if ps.link %} · <a href="{{ ps.link }}">download</a>{% endif %}
    {% if ps.supplement %} · <a href="{{ ps.supplement }}">supplement</a>{% endif %}
    {% if ps.notes %}<div class="pset-notes">{{ ps.notes }}</div>{% endif %}
  </li>
  {% endfor %}
</ul>
{% else %}
<p>No problem sets yet. Add them to <code>_data/psets.yml</code>.</p>
{% endif %}
