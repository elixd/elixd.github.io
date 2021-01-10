---
title: "How to mount Windows shared folder to Linux via Samba"
date: "2019-02-19"
categories: 
  - "linux"
tags: 
  - "linux"
  - "samba"
---

Assuming you have shared the windows folder to local nerwork.

To permanently mount Samba folder into Linux Machine located on the same network. You need:

### Prepare

1.Install CIFS drivers in Linux

```
sudo apt install -y cifs-utils
```

2\. Creare mount point

```
sudo mkdir /media/samba
```

### Mount the Share

#### Add mount config to fstab (Permanent Mount)

```
sudo nano /etc/fstab
```

Add this line to the fstab file:

```
//<ip addresss of windows machine>/<share name> /media/samba cifs user=<user>,pass=<password>,nofail 0 0
```

That's it, not just mount it by running:

```
sudo mount -a
```

#### Temporary Mount

```
mount -t cifs -o username=<username>,pass=<password>,iocharset=utf8,rw,nounix //192.168.0.7/D$ /media/samba
```
