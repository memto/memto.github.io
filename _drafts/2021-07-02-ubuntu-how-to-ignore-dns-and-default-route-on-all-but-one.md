---
layout: post
ads: true
comments: true
published: false
title: Ubuntu - how to ignore DNS and default route on all but one?
categories:
  - linux
  - program
  - tooltips
---
### credit
https://serverfault.com/questions/674731/multiple-dhcp-uplinks-how-to-ignore-dns-and-default-route-on-all-but-one

https://serverfault.com/a/674738

(I am still interested in solutions that only touch /etc/network/interfaces{,.d/}, but not /etc/dhcp/. In the absence of such solutions, I use this one.)

In /etc/dhcp*/dhclient.conf, remove the options routers, domain-name-servers, domain-name, domain-search from the global request statement. Then add this (assuming eth0 is the device where default route and DNS shall not get ignored):

interface "eth0" {
    also request routers, domain-name-servers, domain-name, domain-search;
}
This solution works at least for isc-dhcp-client version 4.2.2.dfsg.1-5+deb70u8 as it is shipped with Debian 7. I assume it works for later versions too.

Edit:

Confirmed that the original idea works with minor changes (eth0 must be quoted, and it should be request, not required)
Specified the dlclient version this works for
