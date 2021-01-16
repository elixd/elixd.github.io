---
title: "ZFS  beginner's notes"
date: "2019-07-27"
---

### Installing on Mac OS

```
brew cask install openzfs
```

Creating a ZPOOL

How to create zpool on a Mac so it can be read on linux (need to disable spacemap\_v2 feature, otherwise it will not be imported in Linux) :

```
sudo zpool create -o feature@spacemap_v2=disabled $POOL_NAME $VDEVS
```

```
# To create a Mac-compatible pool on linux
pool create -o feature@userobj_accounting=disabled usb64 sde
```

Otherwise, is is possible to have a list of compatible features on any system:

```
zpool create -d -o feature@allocation_classes=enabled \
                  -o feature@async_destroy=enabled      \
                  -o feature@bookmarks=enabled          \
                  -o feature@embedded_data=enabled      \
                  -o feature@empty_bpobj=enabled        \
                  -o feature@enabled_txg=enabled        \
                  -o feature@extensible_dataset=enabled \
                  -o feature@filesystem_limits=enabled  \
                  -o feature@hole_birth=enabled         \
                  -o feature@large_blocks=enabled       \
                  -o feature@lz4_compress=enabled       \
                  -o feature@project_quota=enabled      \
                  -o feature@resilver_defer=enabled     \
                  -o feature@spacemap_histogram=enabled \
                  -o feature@spacemap_v2=enabled        \
                  -o feature@userobj_accounting=enabled \
                  -o feature@zpool_checkpoint=enabled   \
                  $POOL_NAME $VDEVS
```

## Setup ZFS on Linux

```
sudo zpool create -o ashift=12 tank mirror sda sdb
# options for perfromance
-o ashift=12,xattr=sa,compression=lz4,atime=off,recordsize=1M

https://icesquare.com/wordpress/how-to-improve-zfs-performance/
https://jrs-s.net/2018/08/17/zfs-tuning-cheat-sheet/
http://open-zfs.org/wiki/Performance_tuning
https://github.com/zfsonlinux/zfs/wiki/FAQ
https://icesquare.com/wordpress/how-to-improve-zfs-performance/
```

Using ZFS

### Dedplication

```
# Turn deduplication on
zfs set dedup=on tank
# Check status of dedudplcation
zfs get dedup tank
# Check dedup ratio for a zvol
zpool list
```





### Snapshots

```
# List snapshots
ls /tank/.zfs/snapshot  # using file system
zfs list -t snapshot    # list zfs filesystems with typy "snapshot"
zfs list -r -t snapshot -o name,creation tank/home

# Check disk spacke taken by snapshots
zfs list -o space

# Create and Manage Snapshots
zfs snapshot -r tank/home@snapshot_name  # "-r" option will make snapshots of all descendent file systems
# Delete snapsot:
zfs destroy tank/home/ahrens@now
# Hold - Prevent snapshot from breing deleted:
zfs hold -r keep tank/home@now
zfs release -r keep tank/home@now

```

## NFS Sharing

Create share:

```
zfs set sharenfs="async,subtree_check,no_root_squash,crossmnt,rw=@192.168.0.0/24" tank/share

# Share with multiple clients:
zfs set sharenfs="rw=@192.168.0.220/24,rw=@192.168.0.217/24" tank2

# Options that work with Nextcloud (exportfs -v): async,wdelay,no_root_squash,sec=sys,rw,secure,no_root_squash,no_all_squash,rw

## MAKE SURE TO END OPTION WITH "rw" AS ABOVE EXAMPLES ##

# Update NFS exports
zfs share tank2/nextcloud
exportfs -ra
```

Enable NFS share:

```
zfs set sharenfs=on tank/share
cat /etc/dfs/sharetab # view shares
exportfs -v #view shares in linux
```

#### View NFS shares

```
zfs get sharenfs # shows nfs settings for ALL zfs filesystems and snapshots
cat /etc/dfs/sharetab # will list shares
exportfs -v # shows shares and their settings

```

[https://blog.programster.org/sharing-zfs-datasets-via-nfs](https://blog.programster.org/sharing-zfs-datasets-via-nfs)

### Mountig NFS via fstab

```
nfs.example.com:/hosts/nextcloud/data /var/www/html/nextcloud/data  nfs4    nosharecache,context="system_u:object_r:httpd_sys_rw_content_t:s0"      0 0
https://www.reddit.com/r/selfhosted/comments/8ashx1/is_it_possible_to_use_an_nfs_share_as_the_main/
```

## ZFS Send / Receive

To send tank to tank2:

```
zfs snapshot -r sourcepool@moving
zfs send -R sourcepool@moving | zfs receive -F destpool
```

Incremental backup:

```
zfs send -R - tank@1 | zfs receive -F tank4/test
zfs send -R -i tank@1 tank@2 | zfs receive -F tank2/test
zfs send -R -i tank/test@2 tank/test@3 | zfs receive -F tank4/test
# Backup from tank2 to tank and monitor progress with PV:
zfs send -R -i tank2@1 tank2@2 | pv | zfs receive -F tank
```

## Backup Tools

[https://github.com/oetiker/znapzend/blob/master/doc/znapzendzetup.pod](https://github.com/oetiker/znapzend/blob/master/doc/znapzendzetup.pod)

[https://github.com/jimsalterjrs/sanoid](https://github.com/jimsalterjrs/sanoid)

```
# Example setup - creat backup plan
znapzendzetup create --recursive --mbuffer=/opt/omni/bin/mbuffer \
>    --mbuffersize=1G --tsformat='%Y-%m-%d-%H%M%S' \
>    --pre-snap-command="/bin/sh /usr/local/bin/lock_flush_db.sh" \
>    --post-snap-command="/bin/sh /usr/local/bin/unlock_db.sh" \
>    SRC '7d=>1h,30d=>4h,90d=>1d' tank/home \
>    DST:a '7d=>1h,30d=>4h,90d=>1d,1y=>1w,10y=>1month' backup/home

# List backup plans
znapzendzetup list

#Run/Start znapzend
znapzend --daemonize
Other options:
znapzend --noaction --debug --runonce=<src_dataset>
znapzend --noaction --debug @
```
