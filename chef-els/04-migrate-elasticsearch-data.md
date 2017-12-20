---
layout: tutorial
title: Migrate Elasticsearch Data
page-type: tutorial
tutorial-category: chef-automate-elasticsearch-cluster
tutorial-order: 04
permalink: /chef-automate-elasticsearch-cluster/migrate-elasticsearch-data
---

Once Elasticsearch is installed and running, we need to migrate our Chef Automate data to one of the Elasticsearch nodes. Once the data has been migrated, we will configure the Elasticsearch cluster which will replicate our data to all Elasticsearch nodes (the next step of this tutorial).

If you have a new Chef Automate instance and don't have any data yopu wish to retain, you may skip this section.

1. Log into the Chef Automate server and create an Elasticsearch snapshot of the built in Elasticsearch instance. The Chef Automate server already comes with a backup job by default. To create an Elasticsearch snapshot, run the following command:

    Note: Ensure that `/var/opt/delivery/elasticsearch_backups/` is empty before running this command.

    ```bash
    curl -X POST 'http://localhost:9200/_snapshot/fs-chef-automate/export_1?wait_for_completion=true'
    ```

2. Copy the backup files that were generated in `/var/opt/delivery/elasticsearch_backups/` to one of your Elasticsearch cluster nodes. For this tutorial, I've placed them in a new folder in `/tmp/elasticsearch_backups/`. Perform the rest of this section on one Elasticsearch node only.

    You can use scp to copy the elasticsearch data with the following command:

    ```bash
    scp -r /var/opt/delivery/elasticsearch_backups/ [YOUR_USER_NAME]@[IP_ADDRESS_OF_CLUSTER_NODE]:/tmp/ 
    ```

3. Change ownership of all of the files in `/tmp/elastcsearch_backups/` to be owned by the `elasticsearch` user:

    ```bash
    sudo chown elasticsearch /tmp/elasticsearch_backups/ -R
    ```

4. Add the following line to `/etc/elasticsearch/elasticsearch.yml` to allow Elasticsearch snapshot imports from this directory:

    ```bash
    path.repo: /tmp/elasticsearch_backups
    ```

5. Restart the elasticsearch service to load the new configuration:

    ```bash
    sudo systemctl restart elasticsearch.service
    ```

6. Register a new snapshot with the following command:

    ```bash
    curl -X PUT 'http://localhost:9200/_snapshot/fs-chef-automate' -d '{"type": "fs", "settings": {"location": "/tmp/elasticsearch_backups", "compress": "true"}}'
    ```

    You should get an acknowledged JSON:

    ```bash
    {"acknowledged":true}
    ```

7. Import Elasticsearch Data via a snapshot

    ```bash
    curl -X POST 'http://localhost:9200/_snapshot/fs-chef-automate/export_1/_restore'
    ```

    You should get an accepted JSON:

    ```bash
    {"accepted":true}
    ```

    You should also see the newly imported indices with the following command:

    ```bash
    curl -X GET 'http://localhost:9200/_cat/indices'
    ```

8. Remove Snapshot Configurations

    1. Remove the snapshot:

    ```bash
    curl -X DELETE 'http://localhost:9200/_snapshot/fs-chef-automate'
    ```

    2. Remove the `path.repo` line from `/etc/elasticsearch.elasticsearch.yml`.