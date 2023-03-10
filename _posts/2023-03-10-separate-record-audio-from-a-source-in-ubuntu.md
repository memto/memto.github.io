---
layout: post
ads: true
comments: true
published: false
title: Separate record audio from a source in Ubuntu
---
#### Create a virtual source

```
pactl load-module module-null-sink sink_name=VirtualOutput1
```
