---
layout: archive
title: "Blog"
permalink: /blog/
author_profile: true
redirect_from:
  - /resume
---
Hi, this is blog!

{% include base_path %}

{% for post in site.teaching reversed %}
  {% include archive-single.html %}
{% endfor %}
