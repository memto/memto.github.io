---
layout: post
title: What Do The Numbers In A Man Page Mean?
published: true
ads: true
comments: true
categories:
  - linux
  - program
  - tooltips
tags:
  - linux
  - man page
  - manual sections
---

The number corresponds to what section of the manual that page is from; 1 is user commands, while 8 is sysadmin stuff. The man page for man itself (man man) explains it and lists the standard ones:

```
MANUAL SECTIONS
    The standard sections of the manual include:

    1      User Commands
    2      System Calls
    3      C Library Functions
    4      Devices and Special Files
    5      File Formats and Conventions
    6      Games et. Al.
    7      Miscellanea
    8      System Administration tools and Deamons

    Distributions customize the manual section to their specifics,
    which often include additional sections.
```    

There are certain terms that have different pages in different sections (e.g. printf as a command appears in section 1, as a stdlib function appears in section 3); in cases like that you can pass the section number to man before the page name to choose which one you want, or use man -a to show every matching page in a row:

```bash
$ man 1 printf
$ man 3 printf
$ man -a printf
```

You can tell what sections a term falls in with man -k (equivalent to the apropos command). It will do substring matches too (e.g. it will show sprintf if you run man -k printf), so you need to use ^term to limit it:

```bash
$ man -k '^printf'
printf               (1)  - format and print data
printf               (1p)  - write formatted output
printf               (3)  - formatted output conversion
printf               (3p)  - print formatted output
printf [builtins]    (1)  - bash built-in commands, see bash(1)
```    

Ref: 
- [What do the numbers in a man page mean? at unix.stackexchange.com](http://unix.stackexchange.com/questions/3586/what-do-the-numbers-in-a-man-page-mean)
