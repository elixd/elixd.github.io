---
title: "Nextcloud"
date: "2019-10-24"
---

### Move Data Directory

[https://help.nextcloud.com/t/howto-change-move-data-directory-after-installation/17170](https://help.nextcloud.com/t/howto-change-move-data-directory-after-installation/17170)

### Tools

Fix permissions

```
chown -R www-data:www-data /var/www/nextcloud
```

Scan for file changes

```
sudo -u www-data php /var/www/nextcloud/occ files:scan --all
```

#### Resolving file locks

For anyone having issues with files getting lock while using redis on nextcloud pi. Specially while syncing nodejs apps…

1. Connect to your redis  
    sudo redis-cli -s /var/run/redis/redis.sock
2. authenticate in your redis with the password for redis found in /var/www/nextcloud/config/config.php  
    $> auth “yourpasswordfoundinconfig.php”  
    $> flushall  
    Your locked files should synchronize now
