---
layout: post
title: Build And Install Apache-Httpd
categories: linux tooltips
tags: apache2 httpd
---


#### Ref:
	https://httpd.apache.org/docs/2.4/install.html#overview

##### My environment
	$ pwd
	/home/dvn000/http
	$ ls
	httpd-2.4.18  httpd-2.4.18.tar.gz
	$ cd httpd-2.4.18
	$ mkdir prefix
	$ ./configure --prefix=/home/dvn000/http/httpd-2.4.18/prefix/


##### configure: error: APR not found.  Please read the documentation.
	
	# Download apr and apr-ultil into srclib
	$ ls srclib/
	apr-1.5.2.tar.gz  apr-util-1.5.4.tar.gz  Makefile.in
	$ cd srclib
	$ tar -xzvf srclib/apr-1.5.2.tar.gz 
	$ mv apr-1.5.2 apr
	$ tar -xzvf apr-util-1.5.4.tar.gz
	$ mv apr-util-1.5.4 apr-util

	# add --with-included-apr option to ./configure apache
	$ ./configure --with-included-apr --prefix=/home/dvn000/http/httpd-2.4.18/prefix/

##### configure: error: pcre-config for libpcre not found. PCRE is required and available from http://pcre.org/
	
	# Download pcre and install
	$ tar -xzvf pcre-8.38.tar.gz
	$ cd pcre-8.38
	$ mkdir prefix
	$ ./configure --prefix=/home/dvn000/http/httpd-2.4.18/pcre-8.38/prefix
	$ make; make install

	# add --with-pcre to ./configure apache
	
	$ ./configure --with-included-apr --with-pcre=/home/dvn000/http/httpd-2.4.18/pcre-8.38/prefix --prefix=/home/dvn000/http/httpd-2.4.18/prefix/
	$ make; make install
	$ cd prefix
	$ ls
	bin  build  cgi-bin  conf  error  htdocs  icons  include  lib  logs  man  manual  modules

##### Start httpd
	$ sudo ./bin/apachectl -k start

AH00558: httpd: Could not reliably determine the server's fully qualified domain name, using 127.0.1.1. Set the 'ServerName' directive globally to suppress this message
	
	# Edit ./conf/httpd.conf
	ServerName localhost

	$ sudo ./bin/apachectl -k restart

Now go to browser, enter localhost into linkbar and see *It works!*

##### Q/A

What is apache MPM (Multi-Processing Modules)
	http://www.vps.net/blog/2013/04/08/apache-mpms-prefork-worker-and-event/#.Vpzw3V4vDH4