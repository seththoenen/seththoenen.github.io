---
layout: tutorial
title: Check Elasticsearch Cluster State
page-type: tutorial
tutorial-category: chef-automate-elasticsearch-cluster
tutorial-order: 06
permalink: /chef-automate-elasticsearch-cluster/check-elasticsearch-cluster-state
---

Once the cluster is configured, we need to ensure it's healthy.

1. Run the following command from one cluster node and make sure all of your cluster nodes show up under `nodes`:

    ```bash
    curl http://localhost:9200/_nodes/process?pretty
    ```

2. If you imported data from Chef Automate, you should see indices returned if you run the following command on each cluster node:

    ```bash
    curl -X GET 'http://localhost:9200/_cat/indices'
    ```