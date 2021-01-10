---
title: "Setup LAMP server on Debian 10 LXC container (Proxmox)"
date: "2019-09-14"
---

## Container configuration

In order to be able to install mariadb, we beed to enable Nesting option in the container after it is created.

## LAMP installation

#### Install required software

```
sudo apt install apache2 mariadb-server mariadb-client php libapache2-mod-php php-mysql
```

#### Scure mariadb

```
sudo mysql_secure_installation
```

That's it. LAMP is installed.

## LAMP Configuration

### Create a database

```
sudo mariadb

MariaDB [(none)]> CREATE DATABASE database_name;
MariaDB [(none)]> GRANT ALL ON example_database.* TO 'user_name'@'localhost' IDENTIFIED BY 'password' WITH GRANT OPTION;
MariaDB [(none)]> FLUSH PRIVILEGES;
MariaDB [(none)]> exit
```

### Set permission for the web dir

```
sudo chown -R www-data:www-data /var/www
```

### Configure Apache - create a Virtual Host (virtual hosts)

#### Configure Virtual Host without a domain

```
sudo nano /etc/apache2/sites-available/your_domain.conf
```

Add aliases for each of your projects located in www dir

```
        Alias /projecta /var/www/projecta
        Alias /projectb /var/www/projectb
        Alias /your_site_name /var/www/your_site_name
```

After that, those sites will be accessible at your\_server\_ip/your\_site name

Site's content needs to be place in corresponding '/var/www/your\_site\_name' dir

#### Configure Virtual Host with a domain

1 - Disable default configuration

```
sudo a2dissite 000-default
```

2 - Add the your\_website configuration

```
<VirtualHost *:80>
    ServerName your_domain
    ServerAlias www.your_domain 
    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/your_domain
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>

# In order to enable .htaccess in WP direcory to take effect:
# Alternatively, paste here contents of .htaccess.
<Directory /var/www/your_domain/>
    AllowOverride All
</Directory>
```

See more about apache2 conf files and .htaccess and how to disable .htaccess for security and performance [here](https://haydenjames.io/disable-htaccess-apache-performance/)

Enable your\_domain config in apache2

```
sudo a2ensite your_domain
```

Test new configuration for syntax (optional)

```
sudo apache2ctl configtest
```

Reload Apache for changes to take effect

```
sudo systemctl reload apache2
```

## SSL

Install Cetrbot and Certbot Apache Module

```
sudo apt install certbot
sudo apt install python-certbot-apache
```

Get the certificate

```
sudo certbot --apache -d your_domain -d www.your_domain
```

This info is taken from [dititalocean](https://www.digitalocean.com/community/tutorials/how-to-secure-apache-with-let-s-encrypt-on-ubuntu-18-04). Also here is an [alternative method](https://websiteforstudents.com/how-to-install-wordpress-with-apache2-and-lets-encrypt-ssl-tls-certificates-on-ubuntu-16-04-18-04/)

Debian 10 related: [digital ocean tutorial](https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mariadb-php-lamp-stack-on-debian-10)
