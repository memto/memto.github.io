---
layout: post
ads: true
comments: true
published: true
title: Separate record audio from a source in Ubuntu
categories:
  - linux
  - tooltips
tags:
  - pavucontrol
  - gnome-sound-recorder
---
#### Create a virtual source

```
$ sudo apt install pavucontrol

$ pactl load-module module-null-sink sink_name=VirtualOutput1

$ sudo apt install gnome-sound-recorder
```


- Create virtual output

![virtual-output.png]({{site.baseurl}}/media/virtual-output.png)


- Playback to virtual output

![playback-to-virtual-output.png]({{site.baseurl}}/media/playback-to-virtual-output.png)


- Record from virtual output (monitor input)

![record-from-virtual-output.png]({{site.baseurl}}/media/record-from-virtual-output.png)
