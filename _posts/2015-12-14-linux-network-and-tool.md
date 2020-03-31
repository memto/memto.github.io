---
layout: post
title: Linux Network And Tool
tags:
  - linux
  - tcpip
  - tcpdump
published: true
ads: true
comments: true
categories:
  - linux
  - tooltips
---

#### Ref:
	http://www.tcpdump.org/tcpdump_man.html
	http://www.cubrid.org/blog/dev-platform/understanding-tcp-ip-network-stack/
	https://wiki.openwrt.org/doc/networking/praxis
	http://bencane.com/2014/10/13/quick-and-practical-reference-for-tcpdump/
	http://dillonhale.com/blog/linux-tutorials/tcpdump-primer/

##### Install wireshark
	$ sudo apt-get install wireshark

Run wireshark
	First, I run it as non-root and can see interface list to capture so I try to run it with sudo
	
	$ sudo wireshark &

	Lua: Error during loading:
 	[string "/usr/share/wireshark/init.lua"]:46: dofile has been disabled due to running Wireshark as superuser. See http://wiki.wireshark.org/CaptureSetup/CapturePrivileges for help in running Wireshark as an unprivileged user.

 	Solve: Setting network privileges for dumpcap
	1. Ensure your linux kernel and filesystem supports File Capabilities and also you have
	installed necessary tools.
	2. sudo setcap 'CAP_NET_RAW+eip CAP_NET_ADMIN+eip' /usr/bin/dumpcap
	3. Start Wireshark as non-root and ensure you see the list of interfaces and can do 
	capture.


##### Install tcpdump
	$ sudo apt-get install tcpdump
