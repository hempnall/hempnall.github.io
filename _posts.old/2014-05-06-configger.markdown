---
layout: post
title: configger 
date: '2014-05-06T14:00:00.001-07:00'
author: James Hook
tags: 
modified_time: '2014-05-06T14:00:56.380-07:00'
visible: 1
---

Something I’ve been meaning to look at is how to parse config files in Linux. The first package a came across was ```libconfig``` – downloadable from ```http://www.hyperrealm.com/libconfig/```. This library can also be used to write config files.

The package can built and installed using the standard ```./configure```, ```make``` and ```sudo make install``` commands. The ```.h``` file gets placed in ```/usr/local/include/libconfig``` and the library files get placed in ```/usr/local/lib```. 

Compiling an empty project in Qt requires the linker directive 

```
LIBS+= -L /usr/local/lib -lconfig++
``` 
It will be necessary to add the lib directory to ```/etc/ld.so.conf``` and run ```sudo ldconfig```.

The library has both a C and C++ API, I’m mostly interested in the C++ API. The web site has a sample config file

```
     version = "1.0";
     
     application:
     {
       window:
       {
         title = "My Application";
         size = { w = 640; h = 480; };
         pos = { x = 350; y = 250; };
       };
     
       list = ( ( "abc", 123, true ), 1.234, ( /* an empty list */) );
     
       books = ( { title  = "Treasure Island";
                   author = "Robert Louis Stevenson";
                   price  = 29.95;
                   qty    = 5; },
                 { title  = "Snow Crash";
                   author = "Neal Stephenson";
                   price  = 9.99;
                   qty    = 8; } );
     
       misc:
       {
         pi = 3.141592654;
         bigint = 9223372036854775807L;
         columns = [ "Last Name", "First Name", "MI" ];
         bitmask = 0x1FC3;
       };
     };

```

A simple Qt source file reads the simple ```version``` and the list and ```misc.columns```

```
#include <QtCore/QCoreApplication>
#include <libconfig.h++>
#include <string>
#include <iostream>

using namespace libconfig;

int main(int argc, char *argv[])
{
    QCoreApplication a(argc, argv);
    Config config;
    std::string s1;

    config.readFile("/home/james/woo.conf");
    config.lookupValue("version",s1);

    Setting& setting = config.lookup("application.misc");
    const char *s2 = setting["columns"][0];

    std::cout << s1 << " " << s2 << std::endl;


    return a.exec();
}

```


