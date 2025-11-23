---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---
{% include base_path %}

{% assign projects = site.projects | where: "project_type", "collaborative" %}
{% for post in projects reversed %}
  {% include archive-single.html %}
{% endfor %}
