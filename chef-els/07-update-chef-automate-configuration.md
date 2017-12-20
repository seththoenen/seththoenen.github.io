---
layout: tutorial
title: Update Chef Automate Configuration
page-type: tutorial
tutorial-category: chef-automate-elasticsearch-cluster
tutorial-order: 07
permalink: /chef-automate-elasticsearch-cluster/update-chef-automate-configuration
---

Now that our Elasticsearch cluster contains our Chef Automate data, we need to point Chef Automate to the cluster and tune the Chef Automate configuration for maximum performance.

1. Update `/etc/delivery/delivery.rb` to point to the Elasticsearch cluster nodes:

    ```bash
    elasticsearch['urls'] = ['http://10.1.1.1:9200', 'http://10.1.1.2:9200', 'http://10.1.1.3:9200']
    ```

2. Disable the local Elasticsearch instance with the following command:

    ```bash
    sudo automate-ctl stop elasticsearch
    ```

3. Reconfigure Chef Automate

    ```bash
    sudo automate-ctl reconfigure
    ```

4. Update the shared count to the number of Elasticsearch cluster nodes:

    ```bash
    curl -XPUT http://localhost:8080/elasticsearch/_template/index_defaults -d '{"template": "*", "settings": {"number_of_shards": 3}}'

    ```

    You should receive JSON output that states the transaction was successful:

    ```bash
    {"acknowledged":true}
    ```

5. Tune LogStash

    1. Create a file at `/etc/security/limits.d/90-chef-performance.conf` to configure limits tuning:

        ```bash
        delivery   soft   nproc   unlimited
        delivery   hard   nproc   unlimited
        delivery   soft   nofile  unlimited
        delivery   hard   nofile  unlimited
        ```

    2. Update `/etc/delivery/delivery.rb` to configure JVM settings, worker threads and batch size:

        ```bash
        logstash['heap_size'] = "2g"
        logstash['config'] = {
          "pipeline" => {
            "batch" => {
              "size" => 40
            },
            "workers" => 16
          }
        }
        ```

6. Cap the RabbitMQ queue to 100,000 with the following steps:

    Note: you may have to elevate to the root user (i.e. `su root`) or log in as root in order for these to complete properly.

    ```bash
    export PATH=/opt/delivery/embedded/bin:$PATH 
    rabbitmqctl set_policy -p /insights max_length '(data-collector)' '{"max-length":100000}' --apply-to queues
    ```