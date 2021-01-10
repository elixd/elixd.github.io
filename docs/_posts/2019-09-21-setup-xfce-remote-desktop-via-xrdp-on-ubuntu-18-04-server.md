---
title: "Setup XFCE remote desktop via XRDP on Ubuntu 18.04 Server"
date: "2019-09-21"
---

```
sudo apt update
sudo apt -y upgrade
sudo apt -y install xfce4 xfce4-goodies xorg chromium-browser
sudo apt -y install xrdp
sudo systemctl enable xrdp
sudo echo 'exec startxfce4' >> /etc/xrdp/xrdp.ini
sudo systemctl restart xrdp

#optional configure ufw
#sudo ufw allow from 192.168.1.0/24 to any port 3389

#Optional stuff
sudo apt install tasksel
sudo tasksel install xubuntu-desktop
```
