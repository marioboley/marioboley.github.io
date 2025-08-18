---
layout: archive
title: "Projects"
permalink: /projects/
author_profile: true
---
{% include base_path %}

Found {{ items | size }} items

Collections: {{ site.collections | map: "label" | join: ", " }}

{% for item in site.projects reversed %}
  {% include archive-single.html %}
{% endfor %}