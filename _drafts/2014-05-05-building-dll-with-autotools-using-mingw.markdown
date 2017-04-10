---
layout: post
title: building dlls with autotools using mingw
date: '2014-05-05T10:32:00.000-07:00'
author: James Hook
tags: 
modified_time: '2014-05-05T11:45:51.350-07:00'
blogger_id: tag:blogger.com,1999:blog-5482187274444893467.post-9126244175200829988
blogger_orig_url: http://computer-says-yes.blogspot.com/2014/05/building-dll-with-autotools-using-mingw.html
visible: 1
---

This one should have been easy - I wanted to compile Linux source code to create Windows DLLs. I thought it would be easy as that was the one job that Libtool has to do right?!?!?

I had used MinGW in the past and new it had its own package management (`mingw-get`) - however, since the last time I had used it it had changed quite a lot. First of all you need to download the Windows Installer from `http://sourceforge.net/projects/mingw/files/`

After running through the wizard I found it best to use the command line version of `mingw-get` (as opposed to the GUI version). You need to install `mingw32-base` and `msys-base` using the GUI. If you launch msys.bat (by default in `c:\MinGW\msys\1.0\msys.bat`) you will find that commands like g++ are missing :-(

In fact mingw-get is missing too.

First of all you have to tell MSYS where where to find these commands. This is done by editing the file `c:\MinGW\msys\1.0\etc\fstab` (create it if necessary) add the following line 

```
C:\MinGW\bin /mingw
```

Restart the msys shell and you should have access to mingw-get. Next you need to install g++ and autotools

mingw-get install gcc g++ mingw32-make autotools


Now you are in a position to start creating your library project. in the project root you will need the following files

```
main.cpp

#include 

int result(int x,int y )  {


 std::cout << x << " + " << y << " = " << (x+y) << std::endl; 
 return x + y;

}
```

```
configure.ac

AC_PREREQ([2.65])

# software name, version, contact address
AC_INIT([math],[1.0.0],[foo.bar@example.com])

# if this file does not exist, `configure` was invoked in the wrong directory
AC_CONFIG_SRCDIR([main.cpp])

# directories (relative to top-level) to look into for AutoConf/AutoMake files
#AC_CONFIG_AUX_DIR([build-aux])
#AC_CONFIG_MACRO_DIR([build-aux])
# enable AutoMake
AM_INIT_AUTOMAKE([1.10 foreign])
# all defined C macros (HAVE_*) will be saved to this file
AC_CONFIG_HEADERS([config.h])

# Macros for the compilers. Check
# http://www.gnu.org/savannah-checkouts/gnu/autoconf/manual/autoconf-2.69/html_node/Compilers-and-Preprocessors.html#Compilers-and-Preprocessors
# for a full list
#
# This macro checks if you have a C compiler
AC_PROG_CC
AM_PROG_CC_C_O

# Check if you have a C++ compiler
AC_PROG_CXX
AC_PROG_CXX_C_O

# Check if you have an Objective-C compiler
# AC_PROG_OBJC
# AC_PROG_OBJC_C_O

# Check for fortran compilers
# AC_PROG_F77
# AC_PROG_F77_C_)
# AC_PROG_FC
# AC_PROG_FC_C_O

# Check if the `install` program is present
AC_PROG_INSTALL

## Initialize GNU LibTool
#
# http://www.gnu.org/software/libtool/manual/html_node/LT_005fINIT.html
#
# GNU LibTool provides a portable way to build libraries.  AutoMake
# knows how to use it; you just need to activate it.
LT_INIT([win32-dll])

# Checks for header files.
#AC_CHECK_HEADERS([stdio.h stdlib.h string.h])

# Checks for typedefs, structures, and compiler characteristics.
AC_TYPE_SIZE_T

# Checks for library functions.
#AC_FUNC_MALLOC
#AC_FUNC_REALLOC
#AC_CHECK_FUNCS([floor pow sqrt])

# Substitute all conditionals in these files; this is normally used to
# create `Makefile`s but could also be used for scripts, include
# files, etc.
AC_CONFIG_FILES([Makefile])

AC_OUTPUT
```


```
Makefile.am

lib_LTLIBRARIES = libmylib.la



libmylib_la_LDFLAGS = -no-undefined
libmylib_la_SOURCES = main.cpp

libmylib_la_CXXFLAGS = --std=c++0x
```


The important line in configure.ac is `LT_INIT([win32-dll])` The important line in `Makefile.am` is ```libmylib_la_LDFLAGS = -no-undefined``` 

Next run the following commands in the project directory

```
$ autoreconf -fi
$ ./configure
$ make
```

And your dll is created!! I'd like to sort out how to install the DLL and its header files by doing make install - but maybe another time lol.
