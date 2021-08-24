---
layout: post
title: ldd Not Show Linked Libs (--no-as-needed)
tags:
  - shared library
  - ldd
published: true
ads: true
comments: true
categories:
  - linux
  - program
---


Problem: gcc build links but shared library does not appear with ldd

Solved:

Most Linux distributions (I assume you are using Linux based on the output of ldd) seem to configure gcc as to pass --as-needed to ld by default (e.g., see here for Debian). This means the final library/executable will only depend on a library (i.e., have a DT_NEEDED tag for that library) if some symbol of that library is actually used by the library/executable.

```bash
$ gcc -Wl,--no-as-needed ...
```

Src: 
- http://stackoverflow.com/questions/28088100/gcc-build-links-but-shared-library-does-not-appear-with-ldd
