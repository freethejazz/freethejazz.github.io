---
layout: default
title: Some Experiments
---

<ul class="posts noList">
  {% for experiment in site.experiments %}
    <li>
      <span class="date">{{ experiment.date | date: '%B %d, %Y' }}</span>
      <h3><a class="post-link" href="{{ experiment.url | prepend: site.baseurl }}">{{ experiment.title }}</a></h3>
      <p class="description">{% if experiment.description %}{{ experiment.description | strip_html | truncate: 250 }}{% else %}{{ experiment.content | strip_html | truncate: 250 }}{% endif %}</p>
    </li>
  {% endfor %}
</ul>
