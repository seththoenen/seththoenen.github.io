---
layout: tutorial
title: Configure Elasticsearch Cluster
page-type: tutorial
tutorial-category: chef-automate-elasticsearch-cluster
tutorial-order: 05
permalink: /chef-automate-elasticsearch-cluster/configure-elasticsearch-cluster
---

Once our Chef Automate data is migrated to one of our Elasticsearch nodes, we need to configure the cluster to replicate data to all nodes.

1. Edit the Elasticsearch configuration file at `/etc/elasticsearch/elasticsearch.yml`:

    1. Un-comment the line that contains `cluster.name:`. Specify a name for your cluster:

        ```bash
        cluster.name: chefels
        ```

    2. Un-comment the line that contains `node.name`. Use the system's hostname for this value:

        ```bash
        node.name: ${HOSTNAME}
        ```

    3. Un-comment the line that contains `bootstrap.memory_lock`. This enables memory locking:

        ```bash
        bootstrap.memory_lock: true
        ```

    4. Un-comment the line that contains `network.host`. Set it to the IP address of the system, and to listen to the loopback address:

        ```bash
        network.host: [ 10.1.1.1, _local_ ]
        ```

    5. Specify cluster node IP addresses. Fine and un-comment the line that contains `discovery.zen.ping.unicast.hosts` and specify the IP address of each cluster node:

        ```bash
        discovery.zen.ping.unicast.hosts: ["10.1.1.1", "10.1.1.2", "10.1.1.3"]
        ```

    6. Specify the minimum master nodes. This should be the (number of cluster nodes) / 2 + 1. I.e., for a 3 node cluster, there should be 2 minimum master nodes. Un-comment the line that contains `discovery.zen.minimum_master_nodes` and set it accordingly:

        ```bash
        discovery.zen.minimum_master_nodes: 2
        ```

2. Edit `/etc/sysconfig/elasticsearch` and un-comment the `MAX_LOCKED_MEMORY` variable:

    ```bash
    MAX_LOCKED_MEMORY=unlimited
    ```

3. Edit `/usr/lib/systemd/system/elasticsearch.service` file and un-comment the `LimitMEMLOCK` variable:

    ```bash
    LimitMEMLOCK=infinity
    ```

4. Edit `/etc/elasticsearch/jvm.options` and set the JVM heap size to 25% of the system's available RAM. By default, this will be set to 2GB in the below lines. Update this to meet your configuration needs:

    ```bash
    -Xms2g
    -Xmx2g
    ```

5. [Ensure the open file descriptor limit is set to 64000+](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-production-elasticsearch-cluster-on-centos-7#configure-open-file-descriptor-limit-(optional)). This should be on by default in CentOS 7. 

6. Restart the Elasticsearch service and ensure it is running:

    ```bash
    sudo systemctl daemon-reload
    sudo systemctl restart elasticsearch.service
    sudo systemctl status elasticsearch.service
    ```

    The service should be `active (running)`. If the service failed to start, something has been misconfigured.

7. Ensure Elasticsearch is still happy:

    ```bash
    curl -X GET 'http://localhost:9200'
    ```

    You should get JSON output similar to the first time you ran this command