---
layout: post
ads: true
comments: true
published: true
title: Graphics Acronyms & What works with what?
categories:
  - linux
  - program
tags:
  - graphic-terms
  - graphic-stack
---
#### Ref
- [Graphics Acronyms & What works with what?](https://www.raspberrypi.org/forums/viewtopic.php?t=96316)

This is in fact fairly interesting question set - probably a source of confusion for many. Therefore some clarifications may be in place... (others feel free to comment, correct and extend - I do not claim to be an expert on this)

Raspberry Pi includes (built into the one 'System On a Chip' (SoC) package) both the ARM 'main processor' / CPU (just like a PC would have one form Intel or AMD) and the VideoCore IV graphics processor / GPU (like a PC would have one from Intel/Nvidia/AMD either built into the motherboard or on a separate graphics card).

The GPU provides hardware accelerated graphics - exposed to application programmers through the standard APIs: OpenGL, OpenVG etc. On the Raspberry Pi the GPU implements the slightly lighter 'mobile device optimised' variant/subset of OpenGL called OpenGLES. OpenGL and OpenGLES are mainly for 3D graphics (can also do 2D) - OpenVG is only for 2D graphics. It should be possible to combine both into same application - at least on two separate graphics surfaces overlaid on top of each other.

'X' (X11, X.org, X Window System) is the 'motor under the hood' of most 'graphical desktop environments' in Linux - it provides the low-level services for input (keyboard, mouse, ...) and output (graphics, sound?, ...). In addition to the 'motor', a typical DTE would also include a window manager and the actual graphical desktop (with user interface widgets etc.). The recommended Linux distribution on Raspberry Pi 'Raspbian' includes a complete set of these components and the current incarnation looks like this - other distributions may have slightly different set of components (different window manager and desktop on top of the common X.org 'motor').

Applications running on the Raspbian desktop can be programmed in many programming languages - some are less 'under-the-hood' concerned than others. Using C it is possible to implement graphical applications linking to the xlib the programming library/API for X.org. Usually one would of course want to use higher level user interface components often referred to as widgets - for this one would use any of the toolkits like GTK, QT(4), SDL, ...) - some possibly requiring C++ as the programming language. There are also other higher level languages like Python (in X one could use PyGTKor Pygame for user interfacing) and Java (OpenJDK 'desktop Java' with AWT/Swing user interface libraries, not 100% sure of the status of this on RPi - better answered on the Java sub-forum I guess), even C# with Mono(?).

The Open* graphics libraries do not provide for display surfaces which (in a windowed desktop environment) one could think of as 'window client areas'. To be able to draw say 3D graphics in an application running within the desktop environment, the program would need to create a desktop window and a graphics surface inside the window - for this there is the GLX library. For Java there is the JOGL library which provides integration with the Open* libraries.

![Linux_Gfx_Stack.png]({{site.baseurl}}/media/Linux_Gfx_Stack.png)
(window manager and desktop libraries omitted (the transparent yellow box) - SDL/GTK/Qt libraries also provide components to use Open* graphics - X does not really know anything about the Open* graphics drawn as the hw composites the X window surface and the GLX surface - at least as far as I know, at the moment the X video driver uses the kernel framebuffer).

However this is a bit different on Raspberry Pi - there is no GLX and a lower level library EGL must be used instead. Also there is the dispmanx library that binds the EGL surface to the actual platform surface.

![RPi_Gfx_Stack_X.png]({{site.baseurl}}/media/RPi_Gfx_Stack_X.png)
(EGL surface is not tied to the X window - the application must move the surface if the window is moved ...which may lead to alignment issues like here with Minecraft)

It is also quite possible to run applications without the desktop environment - without the X window system at all. The simplest graphics can be drawn directly to the framebuffer (see my blog in the signature for examples) - dispmanx and EGL (and through EGL the Open* graphics) can be used from a 'console' application too (see the hello_pi examples provided with Raspbian). These are programmed using C language (may of course be wrapped into other languages). With these libraries accessing the mouse and keyboard is not the easiest and there are no ready made user interface widgets. 

The SDL library can be used without X too - it provides mouse and keyboard handling and other useful functions - there is also a dispmanx backed version of SDL available which provides flicker free double-buffering.

Qt5 (using C++ programming language) on Raspberry Pi provides the full stack of graphics and user interface components. As does the Oracle JDK 8 with JavaFX. One option is to use Python language and the Pi3D library for graphics (not sure what the user interface functionality provided is).

![RPi_Gfx_Stack_noX.png]({{site.baseurl}}/media/RPi_Gfx_Stack_noX.png)
(NOTE: this is not meant to be in anyway complete as it comes to available libraries/frameworks/etc. - ajstarks/openvg and Cogl (RPi port) comes to mind first)

So it all depends on what you want to do, do you need/want to use the desktop environment, do you want to squeeze the last drop of performance, what is your preferred programming language, ...

Where my knowledge really ends short are the different options possibly replacing X or parts of it in the future like Maynard and Wayland(I understand Qt5 would be using Wayland compositor as part of the stack but can Wl be used in other combinations?). By now it is fairly clear that X is somewhat archaic, designed more for network not for desktop, slow on resource constrained devices, etc. but then again it may be around for some time until better options are mature enough.
