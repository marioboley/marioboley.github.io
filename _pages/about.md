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

{% assign recent_posts = site.posts | sort: "date" | reverse %}
<ul>
  {% for post in recent_posts limit:3 %}
    <li>
      <span>{{ post.date | date: "%Y-%m-%d" }}</span>
      &mdash;
      <a href="{{ post.url | relative_url }}">{{ post.title }}</a>
    </li>
  {% endfor %}
</ul>
