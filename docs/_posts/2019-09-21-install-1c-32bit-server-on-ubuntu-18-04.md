---
title: "Install 1C 32bit Server on Ubuntu 18.04"
date: "2019-09-21"
---

```
#Set Russian Locale
sudo update-locale LANG=ru_RU.UTF-8

# Enable 32 bit
dpkg --add-architecture i386
apt-get update

# Install prerequisites 
apt-get install -y unixodbc:i386 imagemagick-6.q16:i386 libgsf-bin:i386 ttf-mscorefonts-installer libcanberra-gtk3-module:i386
apt-get -f install

# Install 1C Server (needs to de downloaded first) in current dir
dpkg -i *.deb
service srv1cv83 start

#Install client - download 32bit deb client
dpkg -i *.deb
apt-get -f install
```
