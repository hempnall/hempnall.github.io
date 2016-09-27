---
layout: post
title: serving secure web pages - part 1 - what is bind? 
date: '2014-05-06T14:00:00.001-07:00'
author: James Hook
tags: 
modified_time: '2014-05-06T14:00:56.380-07:00'
---

This is the first of a series of blog posts about setting up the necessary infrastructure for a secure web server (by which, I mean one that serves web pages using SSL/TLS).

In order to serve secure web pages, you need to have a server certificate that authenticates the web server. The web server is authenticated via its URL or domain name (see later post). To load a web page using its domain name (rather than just its IP address) you must have the web site’s domain name registered on a DNS server somewhere. 

This post shows how to set up Bind (a linux DNS server) to provide name resolution.

# Getting Started

First of all, it is necessary to install bind. On Ubuntu/Debain linux this can be done as follows:

```
# apt-get install bind9 dnsutils
```

Configuration files for bind can be found in ```/etc/bind```.

# Getting into the zone…

The DNS server will define records for zones. These zones can be defined (but don’t have to be defined) in configuration files held in ```/etc/bind/zones/master```.

I will define the zone ```hempnall.com``` on an internal network. Standard practice is to put these in a file called ```db.zone.name```. In this case ```db.hempnall.com```.

```
$TTL 3h
@       IN      SOA     ns1.hempnall.com. admin.hempnall.com.(1,3h,1h,1w,1h)
@       IN      NS      ns1.hempnall.com
www     IN      A       192.168.81.10
ns1     IN      A	   192.168.81.10
```

This is a very minimalist zone file. This file needs to be hooked into the bind service. This is managed by adding the following block to the file ```/etc/bind/named.conf.local```

```
zone "hempnall.com" {
        type master;
        file "/etc/bind/zones/master/db.hempnall.com";
};
```

We should also add a “forward” DNS server in the file ```named.conf.options```

For the zone to take effect we must restart the bind service

```
# /etc/init.d/bind9 restart
```

We can test this with 

```
$ nslookup www.hempnall.com 
```
(set your DNS server in ```/etc/resolv.conf```)


