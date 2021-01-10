---
title: "Run Docker inside XLC container in Proxmox"
date: "2019-02-08"
tags: 
  - "docker"
  - "proxmox"
---

1. Create (preferrably unpriviliged) container
2. Add the below configs to the container conf file:

```
cat <<EOT >> /etc/pve/lxc/101.conf
    #insert docker part below
    lxc.apparmor.profile: unconfined
    lxc.cgroup.devices.allow: a
    lxc.cap.drop:
    EOT
```

3\. Install following apps inside container:

```
apt install wget apparmor
```

Thats It.

Optionally, enable overlay file system (on the PVE host):

```
# add the those lines in /etc/modules-load.d/modules.conf :
aufs
overlay
```
