---
layout: page
title: InSpec for Cloud Platforms
page_type: info
header: yes
permalink: /cloud-inspec/
---

The InSpec for Windows tutorial series will walk you through all of the steps to set InSpec for various cloud providers up on your local Windows 10 machine. No previous experience with InSpec is required, however a familiarity with Ruby is a plus.

{% assign cloudInspecPages = site.pages | where:"tutorial-category", "cloud-inspec" | sort: 'tutorial-order' %}
<ul>
{% for page in cloudInspecPages %}
  <li><a href="{{ page.url | prepend: full_base_url }}">{{ page.tutorial-order }} - {{ page.title | escape }}</a></li>
{% endfor %}
</ul>