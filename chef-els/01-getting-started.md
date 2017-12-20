---
layout: tutorial
title: Getting Started
page-type: tutorial
tutorial-category: chef-automate-elasticsearch-cluster
tutorial-order: 01
permalink: /chef-automate-elasticsearch-cluster/getting-started
---

Before we begin, we are assuming that the following items are already in place:

- You already have a Chef Automate instance populated with run and/or compliance data

- All systems are CentOS 7

- All systems have a static IP address

- All systems are in the same VLAN with the local firewall's disabled

- All systems run on solid state disks

Throughout this tutorial, you will see names and IP addresses that are associated with the three Elasticsearch cluster nodes. For this tutorial, they have the following names and IP addresses:

chefels01 - 10.1.1.1

chefels02 - 10.1.1.2

chefels03 - 10.1.1.3