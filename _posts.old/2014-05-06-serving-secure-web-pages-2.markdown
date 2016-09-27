---
layout: post
title: serving secure web pages
date: '2014-05-06T14:00:00.001-07:00'
author: James Hook
tags: 
modified_time: '2014-05-06T14:00:56.380-07:00'
---

The second part of this series concerns basic setup of an apache server. By basic setup, I don’t mean bundling all your pages in ```/var/www``` and accessing them via ```http://localhost```. I’m aiming for something a bit more scalable; allowing multiple sites (each with different domain names) to be served from the same apache server. The hope is that some of these sites can be served with normal http, and others will be served with https.
To start, install apache:

```
# apt-get install apache2
```

Apache configuration files are stored in ```/etc/apache2```. By default, Apache will server pages out of ```/var/www```.
I’m going to have two web sites:
* ```www.hempnall.com```
* ```secure.hempnall.com```

The pages for these will be contained in the directory structure /var/www in subfolders
* ```www.hempnall.com```
* ```secure.hempnall.com```

I create a file called “index.htm” in each of these folders. The content of this file will be a simple message ```“welcome to <the web site name>!!”```

Changing apache’s configuration..

We can leave apache’s main configuration (apache2.conf) untouched for this exercise. The file of interest is ```sites-enabled/000-default```. By default, this file contains one main configuration block:

```
<VirtualHost *:80>
DocumentRoot /var/www
…
</VirtualHost>
```

In other words, all HTTP sites will be served from ```/var/www```. I will change this to:

```
NameVirtualHost 192.168.81.10:80
<VirtualHost *>
        DocumentRoot /var/www/default
</VirtualHost>
<VirtualHost www.hempnall.com:80>
        ServerName www.hempnall.com
        DocumentRoot /var/www/www.hempnall.com
        DirectoryIndex def.txt
</VirtualHost>
<VirtualHost secure.hempnall.com:80>
        ServerName secure.hempnall.com
        DocumentRoot /var/www/secure.hempnall.com
</VirtualHost>
<VirtualHost *:80>
        DocumentRoot /var/www/
</VirtualHost>
```

In other words, requests coming on IP address ```192.168.81.10``` will be split according to the host: header in the HTTP request. Please note that at this point, the only thing that is secure about secure.hempnall.com is the first bit of the domain name. Restart the server to see effect the changes.


