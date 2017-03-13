---
layout: post
title: Qt - Getting Command Line App Debugging Working
date: '2014-05-18T08:48:00.002-07:00'
author: James Hook
tags: 
modified_time: '2014-05-18T08:48:49.025-07:00'
blogger_id: tag:blogger.com,1999:blog-5482187274444893467.post-1989563686747825293
blogger_orig_url: http://computer-says-yes.blogspot.com/2014/05/qt-getting-command-line-app-debugging.html
visible: 1
---

I've had to do this a number of times. 

The problem is that the terminal window gives the message "Cannot connect creator comm socket" when you try to debug a Command Line application.  

```
$ sudo su -
# echo 0 > /proc/sys/kernel/yama/ptrace_scope
```
and, in QtCreator itself, go to Tools->Options->Environment->General and set the Terminal to be: 

```
/usr/bin/xterm -e
```

```yama/ptrace_scope``` is a kernel configuration setting that controls whether or not one process can see the memory of another process.