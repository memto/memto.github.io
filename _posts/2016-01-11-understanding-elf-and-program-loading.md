---
layout: post
title: Understanding ELF File Format and Program Loading
tags:
  - linux
  - ELF
published: true
ads: true
comments: true
categories:
  - linux
  - program
---


#### Ref:
	http://www.skyfree.org/linux/references/ELF_Format.pdf
	http://www.bottomupcs.com/representing_executables.html

Three main types of object file:

* A *relocation file*

* An *executable file*

* A *shared object file*

#### File format
Object files participate in program linking (building a program) and program execution (running a program).  For convenience and efficiency, the object file format provides parallel views of a file’s contents, reflecting the differing needs of these activities.
![obj-file-format]({{ site.baseurl }}/downloads/draw-hidden/obj-ff.png)
