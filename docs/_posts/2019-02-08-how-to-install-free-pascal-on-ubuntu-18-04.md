---
title: "How to install Free Pascal on Ubuntu 18.04"
date: "2019-02-08"
tags: 
  - "coding"
  - "linux"
---

```
# To install the app:
sudo apt update
apt install fpc

# To prevent "Can't find unit system" error:
sudo ln -s /usr/lib/x86_64-linux-gnu/fpc /usr/lib/fpc
```

Done.

Now run, the editor:

```
fp
```

To complie an app from source code:

```
fpc hello_world.pas
```
