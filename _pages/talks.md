---
layout: page
title: "Talks"
permalink: /talks/
---

This page lists upcoming and past talks. Maintain content in <code>_data/talks.yml</code>.

## Upcoming talks
{% assign upcoming = site.data.talks.upcoming %}
{% if upcoming %}
<ul class="talk-list">
  {% for t in upcoming %}
  <li>
    <strong>{{ t.title }}</strong> — {{ t.speaker }} <span class="muted">({{ t.date }})</span>
    {% if t.location %} · {{ t.location }}{% endif %}
    {% if t.link %} · <a href="{{ t.link }}">details</a>{% endif %}
  </li>
  {% endfor %}
</ul>
{% else %}
<p>No upcoming talks listed.</p>
{% endif %}

## Past talks
{% assign past = site.data.talks.past %}
{% if past %}
<ul class="talk-list">
  {% for t in past %}
  <li>
    <strong>{{ t.title }}</strong> — {{ t.speaker }} <span class="muted">({{ t.date }})</span>
    {% if t.location %} · {{ t.location }}{% endif %}
    {% if t.slides %} · <a href="{{ t.slides }}">slides</a>{% endif %}
    {% if t.video %} · <a href="{{ t.video }}">video</a>{% endif %}
  </li>
  {% endfor %}
</ul>
{% else %}
<p>No past talks listed.</p>
{% endif %}
