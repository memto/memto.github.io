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

- => Nvidia GeForce GTX 1050ti 4GB - NVIDIA Corporation GP107 [GeForce GTX 1050 Ti] - Pascal
- => Info:
	- 

### Install driver
- Option 1: when install OS, or from Software & Updates
![](https://i.imgur.com/MedtrFk.png)

![](https://i.imgur.com/Kb9LXmI.png)

- Option 2: Install CUDA

```bash
# https://forums.developer.nvidia.com/t/cuda-for-geforce-gtx-1050-ti/57928/3
# https://en.wikipedia.org/wiki/CUDA#GPUs_supported

sudo apt-get install linux-headers-$(uname -r)

# Opt1: deb network
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/7fa2af80.pub
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/ /"

# Opt2: deb local
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-ubuntu2004.pin
sudo mv cuda-ubuntu2004.pin /etc/apt/preferences.d/cuda-repository-pin-600
wget https://developer.download.nvidia.com/compute/cuda/11.3.0/local_installers/cuda-repo-ubuntu2004-11-3-local_11.3.0-465.19.01-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu2004-11-3-local_11.3.0-465.19.01-1_amd64.deb
sudo apt-key add /var/cuda-repo-ubuntu2004-11-3-local/7fa2af80.pub

# install (this will also install nvidia driver, so need to remove nvidia*)
sudo apt clean; sudo apt update; sudo apt purge cuda; sudo apt purge nvidia-*; sudo apt autoremove; sudo apt install cuda
```