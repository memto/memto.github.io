---
layout: post
ads: true
comments: true
published: false
title: Ffmpeg custom build on Ubuntu
categories:
  - linux
  - program
  - tooltips
tags:
  - ffmpeg
---

```bash
export FF_SOURCES=/mnt/data/opensource/ffmpeg/ffmpeg_sources
export FF_BUILD=/mnt/data/opensource/ffmpeg/ffmpeg_build
export FF_INSTALL=/mnt/data/opensource/ffmpeg/ffmpeg_install_shared
export NUM_CORES=8
```

## SHARED BUILD
- https://stackoverflow.com/questions/32785279/ffmpeg-doesnt-compile-with-shared-libraries

###=== VIDEO

```bash
cd ${FF_SOURCES} && \
wget -nc https://www.nasm.us/pub/nasm/releasebuilds/2.15.05/nasm-2.15.05.tar.bz2 || echo "existed" && \
tar xjvf nasm-2.15.05.tar.bz2 && \
cd nasm-2.15.05 && \
./autogen.sh && \
PATH="${FF_INSTALL}/bin:$PATH" \
./configure --prefix="${FF_INSTALL}" && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


cd ${FF_SOURCES} && \
git -C x264 pull 2> /dev/null || git clone --depth 1 https://code.videolan.org/videolan/x264.git && \
cd x264 && \
PATH="${FF_INSTALL}/bin:$PATH" \
./configure --prefix="${FF_INSTALL}" \
--enable-shared && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


sudo apt-get install libnuma-dev && \
cd ${FF_SOURCES} && \
git -C x265_git pull 2> /dev/null || git clone https://bitbucket.org/multicoreware/x265_git && \
cd x265_git && git clean -dxf && \
cd build/linux && \
PATH="${FF_INSTALL}/bin:$PATH" \
cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="${FF_INSTALL}" ../../source && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


cd ${FF_SOURCES} && \
git -C libvpx pull 2> /dev/null || git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git && \
cd libvpx && \
PATH="${FF_INSTALL}/bin:$PATH" \
./configure --prefix="${FF_INSTALL}" \
--enable-shared --disable-examples --disable-unit-tests --enable-vp9-highbitdepth --as=yasm && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install
```

###=== AUDIO

```bash
cd ${FF_SOURCES} && \
git -C fdk-aac pull 2> /dev/null || git clone --depth 1 https://github.com/mstorsjo/fdk-aac && \
cd fdk-aac && \
autoreconf -fiv && \
./configure --prefix="${FF_INSTALL}" \
--enable-shared && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


cd ${FF_SOURCES} && \
wget -nc https://downloads.sourceforge.net/project/lame/lame/3.100/lame-3.100.tar.gz || echo "existed" && \
tar xzvf lame-3.100.tar.gz && \
cd lame-3.100 && \
./configure --prefix="${FF_INSTALL}" \
--enable-shared --enable-nasm && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


cd ${FF_SOURCES} && \
git -C opus pull 2> /dev/null || git clone --depth 1 https://github.com/xiph/opus.git && \
cd opus && \
./autogen.sh && \
./configure --prefix="${FF_INSTALL}" \
--enable-shared && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install
```

###=== FFMPEG (Source package vs git: https://stackoverflow.com/a/36904949)

```bash
cd ${FF_SOURCES} && \
git clone https://git.ffmpeg.org/ffmpeg.git || echo "git existed" && \
cd ffmpeg && \
PATH="${FF_INSTALL}/bin:$PATH" \
PKG_CONFIG_PATH="${FF_INSTALL}/lib/pkgconfig" \
./configure --prefix="${FF_INSTALL}" \
  --extra-cflags="-I${FF_INSTALL}/include" \
  --extra-ldflags="-L${FF_INSTALL}/lib" \
  --extra-libs="-lpthread -lm" \
  --ld="g++" \
  --enable-shared \
  --enable-gpl \
  --enable-gnutls \
  --enable-libfreetype \
  --enable-nonfree \
  --enable-libx264 \
  --enable-libx265 \
  --enable-libvpx \
  && echo && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install


  --enable-libass \
  --enable-libfdk-aac \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libvorbis \
  

cd ${FF_SOURCES} && \
wget -nc https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2 || echo "existed" && \
tar xjvf ffmpeg-snapshot.tar.bz2 && \
cd ffmpeg && \
PATH="${FF_INSTALL}/bin:$PATH" \
PKG_CONFIG_PATH="${FF_INSTALL}/lib/pkgconfig" \
./configure --prefix="${FF_INSTALL}" \
  --extra-cflags="-I${FF_INSTALL}/include" \
  --extra-ldflags="-L${FF_INSTALL}/lib" \
  --extra-libs="-lpthread -lm" \
  --ld="g++" \
  --enable-shared \
  --enable-gpl \
  --enable-gnutls \
  --enable-libfreetype \
  --enable-nonfree \
  --enable-libx264 \
  --enable-libx265 \
  --enable-libvpx \
  && echo && \
make -j${NUM_CORES} clean && \
make -j${NUM_CORES} && \
make -j${NUM_CORES} install
```

#### Examples

```bash
cd ${FF_SOURCES}/ffmpeg/doc/examples
make examples
```

#### Issues:
- All configure options come from: ${FF_SOURCES}/ffmpeg/configure

- If any configure failed: check config.log: ${FF_SOURCES}/ffmpeg/ffbuild/config.log

- ERROR: gnutls not found using pkg-config

```bash
https://askubuntu.com/questions/1252997/unable-to-compile-ffmpeg-on-ubuntu-20-04
  > sudo apt-get install libunistring-dev
```

- [libswresample vs. libavresample]

http://ffmpeg.org/pipermail/ffmpeg-devel/2012-April/123746.html