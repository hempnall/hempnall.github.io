---
layout: post
title: Linux Routing
date: '2017-03-30T10:32:00.000-07:00'
author: James Hook
tags: admin linux network
visible: 1
---

# Simple Routing in Mac OS X

Adding a route:

```
$ sudo route -n add -net 192.168.56.0/24  192.168.0.62
```

Deleting a route:

```
$ sudo route -n delete -net 192.168.56.0/24  192.168.0.62
```

# Simple Routing in Linux

Adding a route

```
#
```

Deleting a route
```
# route del -net 192.168.0.0 netmask 255.255.255.0
```

# OpenVPN

Client

```
remote 192.168.0.62
dev tun
ifconfig 192.168.56.3 192.168.56.1
secret static.key
comp-lzo
route 192.168.56.0 255.255.255.0
```

Server

```
dev tun
local 192.168.0.62
ifconfig 192.168.56.1 192.168.56.3
comp-lzo
secret static.key
```

/etc/sysctl.conf
```
net.ipv4.ip_forward=1
```

flush devices
