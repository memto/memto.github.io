---
layout: post
title: Remap Key On Ubuntu
tags:
  - remap-key
published: true
ads: true
comments: true
categories:
  - linux
  - tooltips
---
Remap CAPS_LOCK key to CTRL
```bash
$ sudo vi /etc/default/keyboard
```

then find the line that starts with XKBOPTIONS, and add ctrl:nocaps to make Caps Lock an additional Control key or ctrl:swapcaps to swap Caps Lock and Control.

For example, mine looks like
```
XKBOPTIONS="lv3:ralt_alt,compose:menu,ctrl:nocaps"
```

then run
```bash
$ sudo dpkg-reconfigure keyboard-configuration
```

The reason this way is better is that it will take effect on the virtual consoles (e.g. Ctrl+Alt+F1) as well as in the graphical desktop.

Ref: 
- http://askubuntu.com/questions/149971/how-do-you-remap-a-key-to-the-caps-lock-key-in-xubuntu
