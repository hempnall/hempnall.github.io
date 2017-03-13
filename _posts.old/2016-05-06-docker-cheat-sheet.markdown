---
layout: post
title: Docker Cheat Sheet
date: '2016-05-06T14:00:00.001-07:00'
author: James Hook
tags: 
modified_time: '2016-05-06T14:00:56.380-07:00'
blogger_id: tag:blogger.com,1999:blog-5482187274444893467.post-9215759797196979085
blogger_orig_url: http://computer-says-yes.blogspot.com/2016/05/docker-cheat-sheet.html
visible: 1
---

Loving docker - and wanting to use it as much as possible . But if I don't use it for a week or two then I forget all the commands - so I need to write them down.<br /><br />On Mac OS X you need to use "boot2docker" and access the docker containers through VirtualBox.<br /><br />Dockerfiles are a recipe for creating a new Docker container.

```
FROM ubuntu:14.04
RUN apt-get update && apt-get -y install build-essential python-dev libpython2.7-dev cmake zlib1g-dev flex bison libssl-dev swig libpcap-dev
ADD bro-2.3.1.tar.gz /build
WORKDIR /build/bro-2.3.1
RUN ./configure && make && make install
```

Note:

* It is necessary to run apt-get update .
* ADD'ing a tar ball unzips it when it hits the container
* You need to WORKDIR to set the working directory - RUN'ing cd doesn't work
* first time you use the ubuntu image it downloads it which is time consuming :-(

You can build this using the ```docker build``` command

```
bash-3.2$ docker build -t bro2/v2 .
```
(note: "bro2/v2" is called the tag name)

Running a container:

```
$ docker run -t ubuntu /bin/bash
```
(Note: the -i -t is necessary!! - if you want to interact with your container, also -d to run in daemon mode)

```
bash-3.2$ docker run -t -i hempnall/elk /bin/bash
root@8feca692f88a:/tmp/elasticsearch-1.1.1#
````

Docker attach:

Listing docker images:

```
bash-3.2$ docker images
```
(Note: REPOSITORY: the tag name used in the "build" command)

```
# Delete all containers
docker rm $(docker ps -a -q)

# Delete all images
docker rmi $(docker images -q)
```

Startup apps

Joining ports

Fig

bash-3.2$ boot2docker ip

The VM's Host only interface IP address is: 192.168.59.103

Useful side "things"

supervisor