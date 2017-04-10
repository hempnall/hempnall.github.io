---
layout: post
title: Getting started with Max SDK
date: '2017-04-09T10:32:00.000-07:00'
author: James Hook
tags: creative music
visible: 1
---
# Max SDK

## Download and build..

On the latest version of Mac OS (sierra) I needed to download straight from github. I also needed the correct version of command line tools for Xcode (8.3, available via the Xcode gui)

```
$ git clone  https://github.com/Cycling74/max-sdk
Cloning into 'max-sdk'...
remote: Counting objects: 2725, done.
remote: Total 2725 (delta 0), reused 0 (delta 0), pack-reused 2725
Receiving objects: 100% (2725/2725), 32.82 MiB | 4.68 MiB/s, done.
Resolving deltas: 100% (1004/1004), done.
Checking connectivity... done.
Checking out files: 100% (1750/1750), done.
$
$ cd max-sdk/source
$ ruby ./build.rb
...
takes about 15 minutes
... 
```

## Create a new project...

You will need to make sure your project can see various settings used by xcode.

These are contained in the files:

```
Info.plist
maxmspsdk.xcconfig
```

If you are mimicking the example folder structure, you will need a directory structure such as this:

```
- root 
    - externals
      . ...
      . your external will be created here!
      . ...
    - source 
      . Info.plist
      . maxmspsdk.config
      - sub_folder
        - project_folder
          . your_plugin.c
          . your_plugin.xcodeproj 
```

hence..

```
$ cp -r basics/dummy ~/dev/playpen/basics
$ cp  maxmsdpsdk.config  Info.plist ~/dev/playpen
$ cd ~/dev/playpen/dummy
$ xcodebuild --alltargets clean
$ xcodebuild build
```

## Install into Max ...

Your external plugin will be created in the ```externals``` directory in the root of your SDK folder structure.

To include it into Max, select the File Browser on the left hand side of the Max patcher window (the two concentric circles)

![max file browser](/assets/max_file_browser.png){: class="img-center" }

Select the + button at the bottom of the browser and choose either a single plugin, or add a search path.










