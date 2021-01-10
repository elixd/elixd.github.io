---
title: "Install ONLYOFFICE Document Server for Nextcloud (HTTPS) with Docker"
date: "2019-08-19"
---

```
# Setup Docker container with data stored on external volumes and HTTPS port (443) forwarded to port 44443:
sudo docker run -i -t -d -p 44443:443 --restart=always \
    -v /app/onlyoffice/DocumentServer/logs:/var/log/onlyoffice  \
    -v /app/onlyoffice/DocumentServer/data:/var/www/onlyoffice/Data  \
    -v /app/onlyoffice/DocumentServer/lib:/var/lib/onlyoffice \
    -v /app/onlyoffice/DocumentServer/db:/var/lib/postgresql  onlyoffice/documentserver
```

```
# Generate and copy certificates
openssl genrsa -out onlyoffice.key 2048
openssl req -new -key onlyoffice.key -out onlyoffice.csr
openssl x509 -req -days 365 -in onlyoffice.csr -signkey onlyoffice.key -out onlyoffice.crt
openssl dhparam -out dhparam.pem 2048

mkdir -p /app/onlyoffice/DocumentServer/data/certs
cp onlyoffice.key /app/onlyoffice/DocumentServer/data/certs/
cp onlyoffice.crt /app/onlyoffice/DocumentServer/data/certs/
cp dhparam.pem /app/onlyoffice/DocumentServer/data/certs/
chmod 400 /app/onlyoffice/DocumentServer/data/certs/onlyoffice.key
```

This [article](https://www.linuxbabe.com/ubuntu/integrate-nextcloud-onlyoffice) also offers a good guidance
