---
permalink: /
title: "A/Prof. Mario Boley"
author_profile: true
redirect_from: 
  - /about/
  - /about.html
---

I am Associate Professor at the Department of Information Systems at the University of Haifa working in the area of statistical machine learning, optimisation, and their application to materials science and chemistry. I am also an adjunct at the Department for Data Science and AI at Monash University.

## Updates

{% assign updates = site.publications | concat: site.projects %}
{% assign updates = updates | sort: "date" | reverse %}

<ul>
  {% for item in updates limit:3 %}
    <li>
      <span>{{ item.date | date: "%Y-%m-%d" }}</span>
      &mdash;
      {% if item.collection == "publications" %}[Publication]{% endif %}
      {% if item.collection == "projects" %}[Project]{% endif %}
      <a href="{{ item.url | relative_url }}">{{ item.title }}</a>
    </li>
  {% endfor %}
</ul>
