---
title: "Install mod_security Apache module in Ubuntu 18.04"
date: "2019-10-09"
---

### Install

```
sudo apt-get install modsecurity-crs libapache2-mod-security2
```

Check if it was added to Apache

```
sudo apachectl -M | grep security
```

### Configure

Copy the default configuration:

```
sudo cp /etc/modsecurity/modsecurity.conf-recommended /etc/modsecurity/modsecurity.conf
```

Enable ModSecurity:

```
sudo nano /etc/modsecurity/modsecurity.conf
SecRuleEngine On # change this setting to On
```

Enable for Apache virtual host(s):

```
sudo nano /etc/apache2/sites-available/000-default.conf
```

At the bottom of the file, just above the tag, add the following lines:

```
SecRuleEngine On
```

* * *

### Addition information

```
# Apache configuration file, where modsecurity config is added
/etc/apache2/mods-enabled/security2.conf

# Place where modsecurity rules are stored:
/usr/share/modsecurity-crs/rules/*.conf

# Test if it's working
yoursite_address/?exec=/bin/bash

```

[https://www.linode.com/docs/web-servers/apache-tips-and-tricks/configure-modsecurity-on-apache/](https://www.linode.com/docs/web-servers/apache-tips-and-tricks/configure-modsecurity-on-apache/)

### Exceptions, Whitelisting

[https://www.jasonkingstudios.com/blog/apache-mod\_security-whitelisting-wordpress/](https://www.jasonkingstudios.com/blog/apache-mod_security-whitelisting-wordpress/)

[https://jpmrblood.github.io/apache2/tips/wordpress/modsecurity-wordpress/](https://jpmrblood.github.io/apache2/tips/wordpress/modsecurity-wordpress/)

[https://www.liquidweb.com/kb/whitelisting-in-modsec/](https://www.liquidweb.com/kb/whitelisting-in-modsec/)
