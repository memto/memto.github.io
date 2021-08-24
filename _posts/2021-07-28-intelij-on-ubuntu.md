---
layout: post
ads: true
comments: true
published: true
title: InteliJ on Ubuntu
categories:
  - linux
  - program
  - tooltips
tags:
  - InteliJ
  - desktop-file
---
### Download

- https://www.jetbrains.com/idea/download/#section=linux

![](https://i.imgur.com/xtVPKsQ.png)

- Extract

```bash
tar -xzvf ideaIC-2021.1.3.tar.gz
```

- Create intellij.desktop file

```bash
# create file:
sudo vim /usr/share/applications/intellij.desktop

# add the following
[Desktop Entry]
Version=2021.1.3
Type=Application
Terminal=false

Name=IntelliJ
Icon=<YOUR_PATH>/ideaIC-2021.1.3/idea-IC-211.7628.21/bin/idea.png
Name[en_US]=IntelliJ
Icon[en_US]=<YOUR_PATH>/ideaIC-2021.1.3/idea-IC-211.7628.21/bin/idea.png

Exec=<YOUR_PATH>/ideaIC-2021.1.3/idea-IC-211.7628.21/bin/idea.sh

# mod permissions
sudo chmod 644 /usr/share/applications/intellij.desktop
sudo chown root:root /usr/share/applications/intellij.desktop
```
