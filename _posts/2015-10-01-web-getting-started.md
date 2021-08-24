---
layout: post
title: Learn Web Programming - Getting Started
categories:
  - program
tags:
  - web dev
  - apache
  - php
  - mysql
published: true
ads: true
comments: true
---

Hello web programming!

#### What happened when we enter an URL (a link) into a web browser??

![how-web-work]({{site.baseurl}}/downloads/draw-hidden/how-web-work.png)

<br><br>
##### Step1. The browser contact DNS(Domain Name System) to get IP address of server which serve the site

The chain of events to get the IP address for www.google.com: <br>
Ref: [How DNS lookups work](https://royal.pingdom.com/a-visual-explanation-of-how-dns-lookups-work/ "How DNS lookups work")

(a1) First your browser queries the name server (DNS server) it is setup to use. This is the recursive name server shown above.

> How browser know DNS server IP to query on?? <br>
=> It is provided by DHCP or your manual setup in network setting <br>
Ref: [How browser know DNS server IP](https://serverfault.com/questions/208172/how-does-my-browser-know-where-to-get-data-from-a-server)

The name server doesn’t know the IP address for www.google.com, so it will start the following chain of queries before it can report back the IP address to your browser.

* (a2,a3) Query the Internet root servers to get the name servers for the .com TLD (top level domain).
* (a4,a5) Query the .com TLD name servers to get the authoritative name servers for google.com.
* (a5,a7) Query the authoritative name servers for google.com to finally get the IP address for the host www.google.com
* (a8) Return that IP address to your browser.

##### Step2. When got IP address of site's server (google server), the browser send HTTP request to server to get web pages
What is a web server?

* It just a software that run on a machine and serve HTTP request from client

How a web server work in general? <br>

* It create a socket at specific port (usually on port 80), listen on that port and accept connection from client <br>
* With each connection accepted, web server create a thread to handle user request
* The created thread can return a static web page to user or call a CGI - scripting language to process user data or access database

What is apache? <br>
Ref: [What is an apache?](https://code.tutsplus.com/tutorials/an-introduction-to-apache--net-25786)

* It is one of the most popular web server (of course there is serveral web server software out there)

Why need CGI - scripting language in general?

* To manipulate on user data and/or access database for specific data then return a web page to web server
* If there isn't such CGI functions, a web server can only serve static web pages

What is PHP?

* It is one of scripting language along with others like Perl, Python...

Why need DBMS?

A database is a structured collection of data. Here are some typical examples of databases:

* An online store database that stores products, customer details and orders
* A database for a web forum that stores members, forums, topics and posts
* A database for a blog system, such as WordPress, that stores users, blog posts, categories, tags, and comments

The software that manages databases is known as a database management system, or DBMS. MySQL is an example of a DBMS. Rather confusingly, DBMSs are often called databases too. Strictly speaking though, the database is the data itself, while the DBMS is the software that works with the database.

##### How do Apache, PHP, and MySQL All Work Together? <br>
Ref: [How do apache, PHP and MySQL work together?](https://stackoverflow.com/questions/26132800/how-do-apache-php-and-mysql-all-work-together  "How do apache, PHP and MySQL work together?")  <br>
[How PHP interface with apache](https://stackoverflow.com/questions/2772400/how-does-php-interface-with-apache "How PHP interface with apache")

#### Software stack model <br>
Ref: [LAMP software bundle](https://en.wikipedia.org/wiki/LAMP_(software_bundle) "LAMP software bundle")

So we normally need a web server, a cgi-scripting language, a DBMS work together on a machine. To do so, we can install each separately or install bundle of those software on specific environment. Example of that software bundle is LAMP which original stand for Linux-Apache-MySQL-PHP (if you're on window environment you should instead install WAMP package)

![how-web-work]({{site.baseurl}}/downloads/draw-hidden/LAMP_software_bundle.png)

There are also other software bundle like this. Example is LYME (a solution stack based on Erlang) <br>
Ref: [LYME software bundle](https://en.wikipedia.org/wiki/LYME_%28software_bundle%29 "LYME software bundle")

![how-web-work]({{site.baseurl}}/downloads/draw-hidden/800px-LYME_software_bundle.png)
