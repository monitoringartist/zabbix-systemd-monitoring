Zabbix systemd Monitoring (beta)
================================

If you like or use this project, please provide feedback to author - Star it ★.

Monitoring of systemd units by using Zabbix. Available CPU, mem,
blkio, ... metrics. Compiled [Zabbix Docker shared module](https://github.com/monitoringartist/zabbix-docker-monitoring)
is required. See section [Compilation](https://github.com/monitoringartist/zabbix-docker-monitoring#compilation).
Root permissions are not required.

Module is available as a part of Docker image zabbix-agent-xxl-limited. Quick start:

```
docker run \
  --name=zabbix-agent-xxl \
  -h $(hostname) \
  -p 10050:10050 \
  -v /:/rootfs \
  -v /var/run:/var/run \
  -e "ZA_Server=<ZABBIX SERVER IP/DNS NAME>" \
  -d monitoringartist/zabbix-agent-xxl-limited:latest
```

**Don't use `localhost` or `127.0.0.1` in `ZA_Server` setting!**

Visit [Zabbix agent 3.0 XXL with Docker monitoring](https://github.com/monitoringartist/zabbix-agent-xxl) for more information.

Please donate to author, so he can continue to publish other awesome projects
for free:

[![Paypal donate button](http://jangaraj.com/img/github-donate-button02.png)]
(https://www.paypal.com/cgi-bin/webscr?cmd=_s-xclick&hosted_button_id=8LB6J222WRUZ4)

Available metrics
=================

Note: fid - full unit id (macro *{#FID}*)

| Key | Description |
| --- | ----------- |
| **systemd.discovery[unittype]** | **LLD discovering of running (not all, only running!) systemd units:<br>*unittype* - type of discovered unit: *service, socker, device, mount, automount, swap, target, path, timer, snapshot, slice, scope*<br>Note: Available macros:<br>*{#FID}* - full unit ID (e.g. *systemd-journald.service*)<br>*{#HID}* - human ID (e.g. *journald*) |
| **systemd.mem[fid,mmetric]** | **Memory metrics:**<br>**mmetric** - any available memory metric in the pseudo-file memory.stat, e.g.: *cache, rss, mapped_file, pgpgin, pgpgout, swap, pgfault, pgmajfault, inactive_anon, active_anon, inactive_file, active_file, unevictable, hierarchical_memory_limit, hierarchical_memsw_limit, total_cache, total_rss, total_mapped_file, total_pgpgin, total_pgpgout, total_swap, total_pgfault, total_pgmajfault, total_inactive_anon, total_active_anon, total_inactive_file, total_active_file, total_unevictable*, Note: if you have problem with memory metrics, be sure that memory cgroup subsystem is enabled - kernel parameter: *cgroup_enable=memory* |
| **systemd.cpu[fid,cmetric]** | **CPU metrics:**<br>**cmetric** - any available CPU metric in the pseudo-file cpuacct.stat/cpu.stat, e.g.: *system, user* or container [throttling metrics](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Resource_Management_Guide/sec-cpu.html): *nr_throttled, throttled_time*<br>Note: Jiffy CPU counter is recalculated to % value by Zabbix. |
| **systemd.dev[fid,bfile,bmetric]** | **Blk IO metrics:**<br>**bfile** - container blkio pseudo-file, e.g.: *blkio.io_merged, blkio.io_queued, blkio.io_service_bytes, blkio.io_serviced, blkio.io_service_time, blkio.io_wait_time, blkio.sectors, blkio.time, blkio.avg_queue_size, blkio.idle_time, blkio.dequeue, ...*<br>**bmetric** - any available blkio metric in selected pseudo-file, e.g.: *Total*. Option for selected block device only is also available e.g. *'8:0 Sync'* (quotes must be used in key parameter in this case)<br>Note: Some pseudo blkio files are available only if kernel config *CONFIG_DEBUG_BLK_CGROUP=y*, see recommended docs. |
| **systemd.up[fid]** | **Running state check:**<br>1 if unit is running, otherwise 0 |

Installation
============

* Import provided template [Zabbix-Template-App-systemd-service-simple.xml](https://raw.githubusercontent.com/monitoringartist/zabbix-systemd-monitoring/master/template/Zabbix-Template-App-systemd-service-simple.xml)
or [Zabbix-Template-App-systemd-service-advanced.xml](https://raw.githubusercontent.com/monitoringartist/zabbix-systemd-monitoring/master/template/Zabbix-Template-App-systemd-service-advanced.xml).
* Configure your Zabbix agent(s) - load downloaded/compiled
zabbix_module_docker.so<br>
https://www.zabbix.com/documentation/3.0/manual/config/items/loadablemodules  

Compilation
===========

See section [Compilation](https://github.com/monitoringartist/zabbix-docker-monitoring#compilation).

Troubleshooting
===============

Edit your zabbix_agentd.conf and set DebugLevel:

    DebugLevel=4

Module debug messages will be available in standard zabbix_agentd.log.

Issues and feature requests
===========================

Please use Github issue tracker.

Recommended docs
===============

- https://www.kernel.org/doc/Documentation/cgroups/blkio-controller.txt
- https://www.kernel.org/doc/Documentation/cgroups/memory.txt
- https://www.kernel.org/doc/Documentation/cgroups/cpuacct.txt
- https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Resource_Management_Guide/index.html
- https://www.freedesktop.org/wiki/Software/systemd/
- https://www.digitalocean.com/community/tutorials/understanding-systemd-units-and-unit-files

Author
======

[Devops Monitoring Expert](http://www.jangaraj.com 'DevOps / Docker / Kubernetes / AWS ECS / Zabbix / Zenoss / Terraform / Monitoring'),
who loves monitoring systems, which start with letter Z. Those are Zabbix and Zenoss.

Professional devops / monitoring services:

[![Monitoring Artist](http://monitoringartist.com/img/github-monitoring-artist-logo.jpg)]
(http://www.monitoringartist.com 'DevOps / Docker / Kubernetes / AWS ECS / Zabbix / Zenoss / Terraform / Monitoring')
