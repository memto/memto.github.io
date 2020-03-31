---
layout: post
title: The Struct of A Desktop Environment
categories:
  - linux
tags:
  - linux
  - desktop environment
  - kde
  - gnome
published: true
ads: true
comments: true
---

#### Ref:
- [The Structure of a GUI at www.linux.org](http://www.linux.org/threads/the-structure-of-a-gui.6251/)<br>
- [X Window System at youtube](https://www.youtube.com/watch?v=Y2yVkkBpcw4&index=5&list=PLYNe5sJKU4bIFCGh5xzKBxeODUEdQiZ0Y)<br>
- [Linux Desktop Environments, Window Managers, and Related Topic at youtube](https://www.youtube.com/watch?v=DeO2J3DnZqU&index=1&list=PLYNe5sJKU4bIFCGh5xzKBxeODUEdQiZ0Y)<br>

#### Windowing system (ex: Xorg, SurfaceFlinger, Quartz, XFree86, and Weston)
> http://www.linux.org/threads/display-servers-windowing-systems.6326/

#### Widget-Toolkit (ex: GTK, QT, SDL, AWT, Motif)
> http://www.linux.org/threads/understanding-gtk.6291/

#### Window Manager (ex: Mutter, Metacity, KWin, twm, IceWM)
> http://www.linux.org/threads/various-window-managers.6292/

#### Display Manager (ex: lightdm, kdm(KDE), gdm(GNOME))

#### Desktop
#### Desktop Environment
In summary, a desktop environment is the collection or a bundled package of various GUI components. Each component performs some function in producing a graphical way of interacting with your machine.

- The windowing system (think about Xorg) is the lowest level portion of the GUI that controls the input interaction (mouse and keyboard).

- The window manager puts applications in designated portions of the screen called "windows". Window managers provide a way to change the window size. Users may also use the window manager to close an application.

- The widget toolkits provide a set (predefined) appearance that the window manager should draw. Such toolkits tell the window manager where to place the close, maximize, etc. buttons and how they should appear. Menus are also drawn by window managers after a toolkit declares how the menu should appear.

- Display managers a graphical login interfaces that allows users to login and choose the environment to load (if the user has more than one environment installed).

- Docks and launchers allow users to access certain application and files.

- The desktop is an "invisible" background window that appears to be behind (or at the bottom - below) all of your other windows and docks.
