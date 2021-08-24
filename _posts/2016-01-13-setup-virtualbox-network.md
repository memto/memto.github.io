---
layout: post
title: Setup Virtualbox Network
published: true
ads: true
comments: true
categories:
  - linux
  - tooltips
tags:
  - virtualbox
  - networking
  - NAT
  - Bridge
  - Host-only
---

#### Ref:
> http://bertvv.github.io/notes-to-self/2015/09/29/virtualbox-networking-an-overview/ <br>
https://www.thomas-krenn.com/en/wiki/Network_Configuration_in_VirtualBox#Host-only_Networking <br>


I already setup an ubuntu 14.04 guest in virtualbox, then clone two more of it. 

![vb1]({{ site.baseurl }}/downloads/draw-hidden/vb1.png){: .center-img}


#### Here I will use NAT mode for one interface/adapter (to connect internet) and Host-only mode for another (to setup a connect between host and guests)

To setup NAT mode just select the virtual machine > setting > network > Adapter 1 tab select NAT mode

Next we will apply Host-only mode to Adapter 2, to do that, first we have create a Host-only instance and then apply it to the virtual machine

Step1: File > Preferences > Network > Tab Host-only Networks > Add new host-only instance then edit it

![vb2]({{ site.baseurl }}/downloads/draw-hidden/vb2.png){: .center-img}
![vb3]({{ site.baseurl }}/downloads/draw-hidden/vb3.png){: .center-img}

###### With this setup we will have:

* IP address for the host: 192.168.56.1 (network mask 255.255.255.0)
* DHCP-Server Address: 192.168.56.100
* DHCP-Server Range: 	192.168.56.101 - 192.168.56.254
* The IP range which you can manually set for a guest systems: 192.168.56.2 - 192.168.56.99

Step2: Now you can use that host-only instance for your virtualbox

From virtualbox select the virtual machine then click setting > choose Network

![vb40]({{ site.baseurl }}/downloads/draw-hidden/vb40.png){: .center-img}

Start the virtual machine and set it an IP address

![vb4]({{ site.baseurl }}/downloads/draw-hidden/vb4.png){: .center-img}

#### Now we can ping from host machine to this virtualbox

	$ ping 192.168.56.2
	PING 192.168.56.2 (192.168.56.2) 56(84) bytes of data.
	64 bytes from 192.168.56.2: icmp_seq=1 ttl=64 time=1.91 ms
	64 bytes from 192.168.56.2: icmp_seq=2 ttl=64 time=0.553 ms

#### Apply same to other virtual machine and add it to gnome-connection-manager

![vb5]({{ site.baseurl }}/downloads/draw-hidden/vb5.png){: .center-img}

#### Change ubuntu to text mode (run without gui)
	Ref: http://askubuntu.com/questions/174312/how-can-i-set-my-ubuntu-12-04-lts-to-boot-to-console-without-gui

	Change (edit) in /etc/default/grub

	GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"

	To:

	GRUB_CMDLINE_LINUX_DEFAULT="text"

	Now you must update the grub configs:

	sudo update-grub

	And its done! After reboot, to start the gui just login and type:

	startx
