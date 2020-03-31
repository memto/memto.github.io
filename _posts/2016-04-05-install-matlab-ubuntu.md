---
layout: post
title: 'Install Matlab Ubuntu'
categories: 'linux tooltips'
tags: 'matlab linux'
---


#### Download
	Mathworks_Matlab_R2010a_UNIX_ISO-TBE.5454218.TPB.torrent

#### Install
	Extract or mount downloaded file, go to extracted folder and run:
	$ ./install
	Do as guide in crack/install.txt
	Select path (ex: /opt/matlab2010)

#### Create start file and desktop icon

Because on my system the license server is not auto start so I have to create a script that start license server before start matlab, I called it runmatlab.sh and put it at bin folder in my matlab install path (**/opt/matlab2010/bin/runmatlab.sh**)


	#!/bin/bash

	not_running="$(/opt/matlab2010/etc/lmstat | grep -i 'not running')"
	if [[ not_running ]]; then
	/opt/matlab2010/etc/lmstart
	fi 

	/opt/matlab2010/bin/matlab -desktop

Then you can create start icon on your desktop by create **matlab.desktop** file with contents as below on your desktop

	#!/usr/bin/env xdg-open

	[Desktop Entry]
	Version=1.0
	Type=Application
	Terminal=false
	Exec=/opt/matlab2010/bin/runmatlab.sh
	Name=MATLAB
	Icon=/usr/share/icons/hicolor/48x48/apps/matlab.png
	Categories=Development;Math;Science
	Comment=Scientific computing environment
	StartupNotify=true
	StartupWMClass=com-mathworks-util-PostVMInit
	Path=

Done!
