---
layout: archive
title: "Working Papers"
permalink: /working-papers/
author_profile: true
---

{% include base_path %}

<p style="font-style:italic;color:#666;font-size:0.95em;">
  Note: <sup>+</sup> denotes first / co-first author; <sup>*</sup> denotes corresponding author.
</p>

{% assign sorted_wps = site.working_papers | sort: 'date' | reverse %}
{% for post in sorted_wps %}
  {% include archive-single.html %}
{% endfor %}
