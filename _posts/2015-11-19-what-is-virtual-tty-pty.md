---
layout: post
title: 'What is Virtual TTY, PTY in Linux?'
categories:
  - linux
tags:
  - linux
  - tty
  - pty
published: true
ads: true
comments: true
---

#### Ref:
- [The TTY demystified at linusakesson.net](http://www.linusakesson.net/programming/tty/) <br>
- [How VT-switching works at dvdhrm.wordpress.com](https://dvdhrm.wordpress.com/2013/08/24/how-vt-switching-works/) <br>

#### Switch between Virtual Terminals
```
[Ctrl] + [Alt] + [Fn] key. [Fn] stands for F1 to F12 keys.
```
The same thing can be achieved by using a little command in Linux called chvt.
```bash
$ chvt 7
```

And to return to the 1st virtual terminal, type:
```bash
$ chvt 1
```

Suppose you have Imagemagick installed on your Linux machine and you wish to take a screenshot of your GUI while you are logged in at the console. Here is a simple way of doing it:
```bash
$ chvt 7; sleep 2; import -display :0.0 -window root screenshot-output.png; chvt 1;
```    

What this does is, it first moves to virtual terminal 7, sleeps for 2 seconds, then use the import command to capture the screenshot of the desktop and save it in the file screenshot-output.png, and finally return back to virtual terminal 1 which was your original console. Cool isn’t it?
