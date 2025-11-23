---
layout: archive
title: "Students"
permalink: /students/
author_profile: true
---
{% include base_path %}

{% assign student_projects = site.projects | where: "project_type", "student" %}
{% for post in student_projects reversed %}
  {% include archive-single.html %}
{% endfor %}