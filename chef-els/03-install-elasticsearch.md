---
layout: tutorial
title: Install Elasticsearch
page-type: tutorial
tutorial-category: chef-automate-elasticsearch-cluster
tutorial-order: 03
permalink: /chef-automate-elasticsearch-cluster/install-elasticsearch
---

Now, we need to install Elsticsearch on each of our cluster nodes. For now, we are going to only perform the Elasticsearch install. We will configure the cluster on a later step.

For each Elasticsearch cluster node, do the following:

1. Install Java 8. Java 8 update 141 was the latest version at the time of this writing.

    ```bash
    sudo yum install java-1.8.0-openjdk.x86_64 1:1.8.0.141-1.b16.el7_3
    ```

2. Download Elasticsearch 5.4.1:

    ```bash
    sudo wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.4.1.rpm --no-check-certificate
    ```

3. Install Elasticsearch 5.4.1:

    ```bash
    sudo rpm --install elasticsearch-5.4.1.rpm
    ```

4. Run the following commands to start Elasticsearch:

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl enable elasticsearch.service
    sudo systemctl start elasticsearch.service
    ```

5. Ensure Elasticsearch is running properly with the following command:

    ```bash
    curl -X GET 'http://localhost:9200'
    ```

    Note: it may take a few seconds for Elasticsearch to respond to API requests. 

    You should get output that looks like this:

    ```bash
    {
      "name" : "chefels01",
      "cluster_name" : "chefels",
      "cluster_uuid" : "eUPaWsLRQByAG3QbHGzRBQ",
      "version" : {
        "number" : "5.4.1",
        "build_hash" : "2cfe0df",
        "build_date" : "2017-05-29T16:05:51.443Z",
        "build_snapshot" : false,
        "lucene_version" : "6.5.1"
      },
      "tagline" : "You Know, for Search"
    }
    ```