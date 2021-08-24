---
layout: post
ads: true
comments: true
published: true
title: Ubuntu disk usage cleanup
categories:
  - linux
  - program
  - tooltips
tags:
  - apt-cache
  - log-journal
---
### Ref:
- https://unix.stackexchange.com/questions/130786/can-i-remove-files-in-var-log-journal-and-var-cache-abrt-di-usr
- https://askubuntu.com/questions/65549/var-cache-apt-archives-occupying-huge-space

```bash
sudo apt-get clean
sudo rm -rf /var/log/journal/*
```
