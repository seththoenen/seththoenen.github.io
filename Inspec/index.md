---
layout: page
title: InSpec for Windows
page_type: info
header: yes
permalink: /inspec/
---

The InSpec for Windows tutorial series will walk you through all of the steps to set InSpec up on your local machine and begin auditing systems. No previous experience with InSpec is required, however a familiarity with Ruby is a plus.

As the title states, this tutorial series will focus on Windows. All examples will be ran against a Windows Server 2012 R2 system.

<h3>Getting Started with InSpec</h3>

{% assign inspecPages = site.pages | where:"tutorial-category", "inspec" | sort: 'tutorial-order' %}
<ul>
{% for page in inspecPages %}
  <li><a href="{{ page.url | prepend: full_base_url }}">{{ page.tutorial-order }} - {{ page.title | escape }}</a></li>
{% endfor %}
</ul>
