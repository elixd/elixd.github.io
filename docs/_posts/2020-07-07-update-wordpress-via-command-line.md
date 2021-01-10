---
title: "Update Wordpress via command line"
date: "2020-07-07"
---

## Backup

Backup files:

```
cp -r /var/www/techblog.dudan.ru/var/www/techblog.dudan.ru.backup
```

## Update

### Option 1. Manual Update.

Download latest Wordpress archive and unzip it:

```
wget https://wordpress.org/latest.tar.gz
tar -xzvf latest.tar.gz
```

Sync new files to the the web site folder:

```
rsync -avh --progress wordpress/ techblog.dudan.ru/
```

### Option 2. Update using wp-cli

Install wp-cli:

```
curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
chmod +x wp-cli.phar
sudo mv wp-cli.phar /usr/local/bin/wp
```

Run update:

```
wp core update --path=/var/www/techblog.dudan.ru
```