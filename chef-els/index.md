---
layout: page
title: Chef Automate Elasticsearch Cluster
page_type: info
header: yes
permalink: /chef-automate-elasticsearch-cluster/
---

This guide will walk you through installing and configuring a 3 node Elasticsearch 5 cluster for Chef Automate and is a combination of knowledge from the following sources:

- [Digital Ocean's Guide for a 3-node Elasticsearch 2 Cluster](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-production-elasticsearch-cluster-on-centos-7)

- [Elasticseach documentation on Snapshots](https://www.elastic.co/guide/en/elasticsearch/reference/current/modules-snapshots.html)

- [Scaling Chef Automate Beyond 50,000 Nodes](https://pages.chef.io/rs/255-VFB-268/images/ScalingChefAutomate_2017.pdf)

- My own experience administering Chef infrastructure

<h3>Contents</h3>

{% assign chefAutomateElasticPages = site.pages | where:"tutorial-category", "chef-automate-elasticsearch-cluster" | sort: 'tutorial-order' %}
<ul>
{% for page in chefAutomateElasticPages %}
  <li><a href="{{ page.url | prepend: full_base_url }}">{{ page.tutorial-order }} - {{ page.title | escape }}</a></li>
{% endfor %}
</ul>