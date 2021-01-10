---
title: "Mikrotik: How to make your self-hosted web site available both on the WEB and on the LAN"
date: "2020-07-06"
---

### 1\. Firewall: allow requests to ports 80 and 443

```
/ip firewall filter
add action=accept chain=forward dst-port=80 in-interface=ether1 protocol=tcp comment="allow requests to port 80 to be forwarded (to web server)"
```

Repeat for port 443 if needed

### 2\. Configure NAT

#### Configure port forwarding from WEB to the LAN server:

```
/ip firewall nat add chain=dstnat in-interface=ether1 protocol=tcp dst-port=80 action=dst-nat to-addresses=192.168.0.9 to-ports=80 comment="forward from wan to web server port 80"
```

Do the same for 443 port if needed

#### Configure address substitution (explained [here](https://wiki.mikrotik.com/wiki/Hairpin_NAT)):

```
/ip firewall nat
add chain=srcnat src-address=192.168.0.0/24 dst-address=192.168.0.9 protocol=tcp dst-port=80 out-interface=ether1 action=masquerade
```

Do the same for 443 port if needed
