---
title: "Install Docker on Debian 9"
date: "2019-02-19"
categories: 
  - "scripts"
tags: 
  - "docker"
  - "linux"
---

Here is a simple script to install docker on Debian 9

1. Create the script text file with nano

```
sudo nano install_docker.sh
```

2\. Copy this code to the file and save it

```
#!/bin/bash 
# Run this script to install Docker on Debian 9

#Install prerequisites
sudo apt update
sudo apt --yes install apt-transport-https ca-certificates curl gnupg2 software-properties-common

# Add the GPG key for the official Docker repository to your system:
curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -

# Add the Docker repository:
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable"

# Istall Docker
sudo apt update
sudo apt install -y docker-ce docker-ce-cli containerd.io

#Install docker-compose
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

echo "---------------- INSTALLATION FINISHED ---------------"

# Check docker status
sudo systemctl status docker
```

3\. Make it executable:

```
chmod +x install_docker.sh
```

4\. Run the script

```
sudo ./install_docker.sh
```

P.S. it might work on other systems aside fro Debian 9, but tested on Debian 9. This script is intended to simplify Docker setup in case I need it in the future.
