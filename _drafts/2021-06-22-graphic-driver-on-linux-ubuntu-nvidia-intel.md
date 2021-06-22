---
layout: post
ads: true
comments: true
published: false
title: Graphic driver on Linux/Ubuntu (Nvidia/Intel)
---
### Ref:
- https://wiki.debian.org/HardwareVideoAcceleration
- https://wiki.archlinux.org/title/Hardware_video_acceleration
- //
- http://lifestyletransfer.com/how-to-install-nvidia-gstreamer-plugins-nvenc-nvdec-on-ubuntu/
- http://lifestyletransfer.com/how-to-install-gstreamer-vaapi-plugins-on-ubuntu/
- https://tome.one/playing-10bit-hevc-videos-on-linux-with-nvidia-and-mpv.html

### Get information
```bash
sudo lspci -vnn | grep VGA
sudo lshw -c video
sudo lshw -c display
```

=> Nvidia GeForce GTX 1050ti 4GB - NVIDIA Corporation GP107 [GeForce GTX 1050 Ti] - Pascal

### Install driver

![](https://i.imgur.com/Kb9LXmI.png)