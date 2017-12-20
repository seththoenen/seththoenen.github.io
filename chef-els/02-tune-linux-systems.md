---
layout: tutorial
title: Tune CentOS 7 Systems
page-type: tutorial
tutorial-category: chef-automate-elasticsearch-cluster
tutorial-order: 02
permalink: /chef-automate-elasticsearch-cluster/tune-centos7-systems
---

Before we begin installing and configuring Elasticsearch and Chef Automate, we need to tune the CentOS 7 configuration in order to get the best performance out of your Elasticsearch cluster and Chef Automate instance.

For each system (Chef Automate and the Elasticsearch cluster nodes), do the following:

1. Set SELinux to permissive

    ```bash
    setenforce 0
    sed -i 's/SELINUX=enforcing/SELINUX=permissive/g' /etc/selinux/config
    ```

2. Configure RHEL boot-time kernel tuning by adding the following arguments to the `GRUB_CMDLINE_LINUX` variable in `/etc/default/grub`:

    ```bash
    scsi_mod.use_blk_mq=Y elevator=noop transparent_hugepage=never
    ```

3. Tune Linux kernel VM tuning by creating a file at `/etc/sysctl.d/chef-highperf.conf` and populate with the following contents:

    ```bash
    vm.swappiness=10
    vm.max_map_count=262144
    vm.dirty_ratio=20
    vm.dirty_background_ratio=30
    vm.dirty_expire_centisecs=30000 
    ```

4. Ensure all filesystems are XFS (skipped in this tutorial)

5. Reboot