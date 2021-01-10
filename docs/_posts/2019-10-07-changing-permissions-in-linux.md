---
title: "Changing permissions in linux"
date: "2019-10-07"
---

### Glossary

#### Permission types

```
r = read, w = write, x = execute, X = execute (applies only to folders)
u = user, g = group, o = other, a = ugo = "all"
```

#### Numerical permissions

```
Number	Permission Type                Symbol
0	No Permission	               ---
1	Execute	                       --x
2	Write	                       -w-
3	Execute + Write	               -wx
4	Read	                       r--
5	Read + Execute	               r-x
6	Read + Write                   rw-
7	Read + Write +Execute          rwx
```

### chmod command - changing permission

```
# Examples:
chmod u=rwX,g=rX,o=rX *    # file = rw,r,r; folder = rwx,rx,rx
chmod u=rwX,go=rX *.       # same as above
chmod a+r *                # add read permission for all
chmod o-r *                # remove read permission for others
chmod -R a+X *             # add execute permission fo folders recursively

# Wordpress example: set files = 444 and folders to 755:
find /var/www/site_name -type f -exec chmod 444 {} + -o -type d -exec chmod 755 {} +
# I this case you may also want to set ownership:
sudo chown -R www-data:www-data /var/www
```

### Viewing file permission in Linux

```
ls -al
```
