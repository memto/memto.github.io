---
layout: post
ads: true
comments: true
published: true
title: Building QT from source Ubuntu 16.04 and link from QtCreator
categories:
  - linux
  - program
  - tooltips
tags:
  - qt5
  - qt5-build
---
#### Ref/Credit
- http://wiki.qt.io/Building_Qt_5_from_Git#Getting_the_source_code

#### System Requirements
- Git (>= 1.6.x)			(git --version)
- Perl (>=5.14) 			(perl -v)
- Python (>=2.6.x)			(python --version)
- A working C++ compiler



```bash
## Linux/X11
sudo apt-get build-dep qt5-default
sudo apt-get install libxcb-xinerama0-dev

## Build essential
sudo apt-get install build-essential perl python git

## Libxcb
sudo apt-get install '^libxcb.*-dev' libx11-xcb-dev libglu1-mesa-dev libxrender-dev libxi-dev

## QT webengine
sudo apt-get install libssl-dev libxcursor-dev libxcomposite-dev libxdamage-dev libxrandr-dev libdbus-1-dev libfontconfig1-dev libcap-dev libxtst-dev libpulse-dev libudev-dev libpci-dev libnss3-dev libasound2-dev libxss-dev libegl1-mesa-dev gperf bison

## QT multimedia
## Issue: The QMediaPlayer object does not have a valid service
## Please check the media service plugins are installed.
sudo apt-get install libasound2-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
```

#### Geting Source code
```bash
git clone https://code.qt.io/qt/qt5.git
cd qt5
git checkout 5.10
```

- Initialize the repository using the init-repository script

```bash
  Module options:

    --module-subset=<module1>,<module2>...
        Only initialize the specified subset of modules given as the
        argument. Specified modules must already exist in .gitmodules. The
        string "all" results in cloning all known modules. The strings
        "essential", "addon", "preview", "deprecated", "obsolete", and
        "ignore" refer to classes of modules; "default" maps to
        "essential,addon,preview,deprecated", which corresponds with the
        set of maintained modules and is also the default set. Module
        names may be prefixed with a dash to exclude them from a bigger
        set, e.g. "all,-ignore".
```

```bash
perl init-repository --module-subset=essential
```

#### Configuring and Building
```
export LLVM_INSTALL_DIR=/usr/llvm
./configure -developer-build -opensource -nomake examples -nomake tests -prefix <PREFIX-PATH> -debug -recheck-all -confirm-license
```

#### Link to the built from QtCreator

#### Issue
- QFontDatabase: Cannot find font directory \<PREFIX-PATH\>/lib/fonts.
	- Description
    
    ```
    QFontDatabase: Cannot find font directory <PREFIX-PATH>/lib/fonts.
    Note that Qt no longer ships fonts. Deploy some (from http://dejavu-fonts.org for example) or switch to fontconfig.
    ```
    
	- Fix:
    
    ```
    Create required folder and put some fonts there
    ```