---
layout: default
title: 목차
---

# Data Science 학습 자료

Data Science 기초부터 머신러닝, 통계분석까지 단계별 학습 자료입니다.

<ul class="toc-list">
{% assign chapters = site.chapters | sort: 'order' %}
{% for c in chapters %}
  <li>
    <a href="{{ c.url | relative_url }}">{{ c.order }}. {{ c.title }}</a>
    {% if c.description %}<p>{{ c.description }}</p>{% endif %}
  </li>
{% endfor %}
</ul>
