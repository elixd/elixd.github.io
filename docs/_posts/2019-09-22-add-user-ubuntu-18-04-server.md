---
title: "Add User Ubuntu 18.04 Server"
date: "2019-09-22"
---

```
useradd -m username # is to create home dir
passwd username 
usermod -aG sudo username #add user to sudoers
```

```
#Edit /etc/passwd and make sure thing in your userline is /bin/bash and not /bin/sh
```
