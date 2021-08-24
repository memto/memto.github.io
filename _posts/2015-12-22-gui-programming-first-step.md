---
layout: post
title: Gui Programming First Step
tags:
  - Gui
  - Gui-toolkit
published: true
ads: true
comments: true
categories:
  - program
---

#### Core Language vs GUI toolkit???
	http://www.tldp.org/HOWTO/Programming-Languages-3.html
	http://www.nairaland.com/1562329/hope-c-newbies-gui-toolkits
	Extra:
	1) yes, Win32 is an API for C, Win32 API is part of Windows Operating system.
	2) yes, MFC is the wrapper for Win32 API to make GUI development easier.
	3) yes, .NET is different from MFC and Win32 API as the code written in .NET languages is managed one ( means CLR is taking responsibility) but in Win32 and MFC it is unmanaged. you can also write unmanaged code in .NET.

	C++ using .NET is called manged C++ and its different from C++ using MFC (unmanaged C++)

	.NET CLR uses Win32 API (and may be some assembly code for performance improvement). in fact both MFC and .NET use Win32, like in this relationship

	MFC --> Win32 --> Assembly Code
	.NET --> Win32 --> Assembly Code

	for further study , see links

	http://www.go4expert.com/forums/showthread.php?t=2533    (Win32 vs MFC)

	http://www.daniweb.com/forums/thread198090.html   (MFC vs .NET)

Core Language: is the syntax and construct of a programming language itself such as: struct, class, pointer, loop, branch...

GUI toolkit: is a lib that provide API for developer to develop GUI apps on specific OS/System or cross-platform for specific programming language such as wxWidget/QT for C++; SWT/Swing for Java

#### GUI programming with python

As said above, we need a GUI toolkit to start build an GUI app. With python there are few of them exist such as: Tkinter, Kivy, PyQt, PyGUI, wxPython. After searching for awhile on the internet I decided to choose wxPython for my project

###### Install wxPython on linux
	sudo apt-get install python-wxgtk2.8 python-wxgtk2.8-dbg

###### API Docs
	http://xoomer.virgilio.it/infinity77/wxPython/index.html

###### Tutorial
	http://zetcode.com/wxpython/firststeps/
