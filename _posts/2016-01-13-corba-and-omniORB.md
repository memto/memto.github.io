---
layout: post
title: Corba And OmniORB
tags:
  - Corba
  - Corba
published: true
ads: true
comments: true
categories:
  - program
---

#### Ref:
	https://en.wikipedia.org/wiki/Common_Object_Request_Broker_Architecture
	http://omniorb.sourceforge.net/docs.html
	http://www.yolinux.com/TUTORIALS/CORBA.html


#### What is the CORBA?
If you want to call a function from remote node (ex. run on other computer via network) or call a function which is implemented in different programming language, same as when you want to talk with person who say different language, you will need a middle man to help you can communicate. And the CORBA's function here is the same as the middle man.

![corba-call-seq]({{ site.baseurl }}/downloads/draw-hidden/CORBA-call-sequence.png)

As you can see from the above figure, you can think CORBA's functions is same as a DNS server. You can register an Object to it then programs from remote node can come to it and ask for address of the Object, call a function on that Object which is exported via an interface that known by both side (IDL interface). Also same as there are many of middle man out three, there are several implementations of CORBA and omniORB is one of that implementations. In this post, I'll show you how to install omniORB and run some example from the source code to see how it work from the action.

#### Download and install
You can download the omniORB from here:

	http://sourceforge.net/projects/omniorb/files/

Steps to install it are quite easy, cd to folder the source is downloaded into and run below commands

	$ tar -xjvf omniORB-4.2.0.tar.bz2
	$ cd omniORB-4.2.0/
	$ ./configure
	$ make -j4
	$ sudo make install

Build examples from the source code

	$ cd src/examples/
	$ make

To understand some terms and examples you should read this document

	http://omniorb.sourceforge.net/omni42/omniORB/omniORB002.html

Run omniname which is omniORB's middle man, this is my run_omniname9100.sh script you should read it and the document then modify some to have it runs on your system

	Ref: http://omniorb.sourceforge.net/omni42/omniNames.html

	# run_omniname9100.sh
	RUN_DIR=/home/msc/netxms/omninames/9100

	rm -f $RUN_DIR/*.bak
	rm -f $RUN_DIR/*.dat
	rm -f $RUN_DIR/*.dat.log

	export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/lib
	export OMNINAMES_LOGDIR=$RUN_DIR

	cd /usr/local/bin
	#./omniNames -ORBendPointPublish giop:tcp:192.168.1.210: -ORBtraceLevel 25 -start 9100 &
	./omniNames -start 9100 &


#### Run examples:
First, we have to setup an omni-config-file which specify the omniname's address:

	# omni_app.cfg
	InitRef=NameService1=corbaloc:iiop:10.61.63.123:9100/NameService
	#InitRef=NameService2=corbaloc:iiop:10.61.63.123:9200/NameService
	#InitRef=NameService2=corbaloc:iiop:10.61.63.123:9100/NameService

	#endPoint = giop:tcp:10.61.63.123:8000-10000

Then export it to our program (of course, you have to know the middle man's address, right?)

	$ export OMNIORB_CONFIG=/opt/mssng/oam/etc/omni_app.cfg

Show log when run app

	$ export ORBtraceLevel=25

Here I just copy echo.idl and eg3 from example and modify it for testing

	$ mkdir testomniORB; cd testomniORB
	$ cp omniORB-4.2.0/idl/echo.idl
	$ omniidl -bcxx echo.idl
	$ g++ ex3_impl_mulinst.cc echoSK.cc -lomniORB4 -lomnithread -o ex3_impl_mulinst
	$ g++ ex3_clt_mulinst.cc echoSK.cc -lomniORB4 -lomnithread -o ex3_clt_mulinst


{% gist ab8e4363b15395a68086 %}
