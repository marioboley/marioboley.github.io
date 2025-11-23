---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---
{% include base_path %}

{% assign research = site.projects | where:"project_type","collaborative" %}
{% for item in research %}
  {% include archive-single.html %}
{% endfor %}
