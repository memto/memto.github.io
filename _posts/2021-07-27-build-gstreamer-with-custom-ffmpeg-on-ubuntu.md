---
layout: post
ads: true
comments: true
published: true
title: Build gstreamer with custom Ffmpeg on Ubuntu
categories:
  - linux
  - program
  - tooltips
tags:
  - gstreamer
  - ffmpeg
---

#### gstreamer source git

```bash
https://gitlab.freedesktop.org/gstreamer/gstreamer
https://gitlab.freedesktop.org/gstreamer/gst-plugins-base
https://gitlab.freedesktop.org/gstreamer/gst-plugins-good
https://gitlab.freedesktop.org/gstreamer/gst-plugins-ugly
https://gitlab.freedesktop.org/gstreamer/gst-plugins-bad
https://gitlab.freedesktop.org/gstreamer/gst-rtsp-server
```

#### Build Ffmpeg (shared)
- [Ffmpeg custom build (shared) on ubuntu](https://memto.github.io/linux/program/tooltips/2021/07/27/ffmpeg-custom-build-on-ubuntu/)


#### Build Gstreamer with custom Fffmpeg build

```bash
export FF_INSTALL=/mnt/data/opensource/ffmpeg/ffmpeg_install_shared
export GS_SOURCES=/mnt/data/opensource/gstreamer/gstreamer_sources
export GS_INSTALL=/mnt/data/opensource/gstreamer/gstreamer_install
export NUM_CORES=8
export GS_VER=1.14.5
```

```bash
cd ${GS_SOURCES} && \
git clone https://gitlab.freedesktop.org/gstreamer/gstreamer || echo "existed" && \
cd gstreamer && \
git checkout 1.14.5 -b 1.14.5 || echo "branch existed" && \
PATH="${GS_INSTALL}/bin:$PATH" \
./autogen.sh --prefix="${GS_INSTALL}" \
--disable-gtk-doc && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


cd ${GS_SOURCES} && \
git clone https://gitlab.freedesktop.org/gstreamer/gst-plugins-base || echo "git existed" && \
cd gst-plugins-base && \
git checkout 1.14.5 -b 1.14.5 || echo "branch existed" && \
PATH="${GS_INSTALL}/bin:$PATH" \
export PKG_CONFIG_PATH="${GS_INSTALL}/lib/pkgconfig:${FF_INSTALL}/lib/pkgconfig:$PKG_CONFIG_PATH" && \
./autogen.sh --prefix="${GS_INSTALL}" \
--disable-gtk-doc && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


cd ${GS_SOURCES} && \
git clone https://gitlab.freedesktop.org/gstreamer/gst-plugins-good || echo "git existed" && \
cd gst-plugins-good && \
git checkout 1.14.5 -b 1.14.5 || echo "branch existed" && \
PATH="${GS_INSTALL}/bin:$PATH" \
export PKG_CONFIG_PATH="${GS_INSTALL}/lib/pkgconfig:${FF_INSTALL}/lib/pkgconfig:$PKG_CONFIG_PATH" && \
./autogen.sh --prefix="${GS_INSTALL}" \
--disable-gtk-doc && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


cd ${GS_SOURCES} && \
git -C gst-plugins-ugly pull 2> /dev/null || git clone https://gitlab.freedesktop.org/gstreamer/gst-plugins-ugly || echo "git existed" && \
cd gst-plugins-ugly && \
git checkout 1.14.5 -b 1.14.5 || echo "branch existed" && \
PATH="${GS_INSTALL}/bin:$PATH" \
export PKG_CONFIG_PATH="${GS_INSTALL}/lib/pkgconfig:${FF_INSTALL}/lib/pkgconfig:$PKG_CONFIG_PATH" && \
./autogen.sh --prefix="${GS_INSTALL}" \
--disable-gtk-doc && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


cd ${GS_SOURCES} && \
git -C gst-plugins-bad pull 2> /dev/null || git clone https://gitlab.freedesktop.org/gstreamer/gst-plugins-bad || echo "git existed" && \
cd gst-plugins-bad && \
git checkout 1.14.5 -b 1.14.5 || echo "branch existed" && \
PATH="${GS_INSTALL}/bin:$PATH" \
export PKG_CONFIG_PATH="${GS_INSTALL}/lib/pkgconfig:${FF_INSTALL}/lib/pkgconfig:$PKG_CONFIG_PATH" && \
./autogen.sh --prefix="${GS_INSTALL}" \
--disable-gtk-doc && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


cd ${GS_SOURCES} && \
git -C gst-rtsp-server pull 2> /dev/null || git clone https://gitlab.freedesktop.org/gstreamer/gst-rtsp-server || echo "git existed" && \
cd gst-rtsp-server && \
git checkout 1.14.5 -b 1.14.5 || echo "branch existed" && \
PATH="${GS_INSTALL}/bin:$PATH" \
export PKG_CONFIG_PATH="${GS_INSTALL}/lib/pkgconfig:${FF_INSTALL}/lib/pkgconfig:$PKG_CONFIG_PATH" && \
./autogen.sh --prefix="${GS_INSTALL}" \
--disable-gtk-doc && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install
```
