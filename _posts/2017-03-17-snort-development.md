---
layout: post
title: snort development
date: '2017-03-17T10:32:00.000-07:00'
author: James Hook
tags: qwe
modified_time: '2017-05-05T11:45:51.350-07:00'
visible: 1 
---

# Setting up the dev environment

I used Ubuntu 16.04. Install the following packages and then compile snort.

```
$ sudo apt-get install \
    flex \
    bison \
    zlib1g-dev \
    libdumbnet-dev \
    libdaq-dev \
    libpcre3-dev \
    libpcap-dev
...
$ cd snort-2.9.9.0
$ ./configure --enable-build-dynamic-examples
$ make
...
$
```

# Playing with Dynamic examples

We need to setup the preprocessor in the ```snort.conf``` in ```./etc/```

```
dynamicpreprocessor directory src/dynamic-preprocessors/build/usr/local/lib/snort_dynamicpreprocessor/
# add our new preprocessor!!!!
dynamicpreprocessor directory src/dynamic-examples/dynamic-preprocessor/.libs/lib_sfdynamic_preprocessor_example.so
dynamicengine src/dynamic-plugins/sf_engine/.libs/libsf_engine.so
dynamicdetection directory src/dynamic-examples/dynamic-rule
...
preprocessor dynamic_example: port 80
```

I also commented out the setup of the ```reputation``` preprocessor along with all the rule includes (they aren't relevant today)


