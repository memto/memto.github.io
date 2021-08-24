---
layout: post
ads: true
comments: true
published: true
title: Two network interfaces on Ubuntu
categories:
  - linux
  - program
  - tooltips
tags:
  - default gateway
  - linux routing
---
#### Ref
- https://www.ni.com/en-vn/support/documentation/supplemental/11/best-practices-for-using-multiple-network-interfaces--nics--with.html

#### Usage case

![](https://i.imgur.com/qw5Y8sQ.png)

- Have two network interfaces on my PC: 1 to local network (**eno1**) and 1 to public network (**enx_fe**) as above
- Want to use **eno1** for local trafic and **enx_fe** for internet access

#### solution

- Delete local defaul gateway + add route for local network

```bash
$ sudo ip route del default via 10.150.124.1 dev eno1
$ sudo route add -net 10.0.0.0/8 gw 10.150.124.1 dev eno1
```

- This means that: use **eno1** for trafic on **10.0.0.0/8** network and **enx_fe** for all others
