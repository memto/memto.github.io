---
layout: post
ads: true
comments: true
published: true
title: ' Ffmpeg custom build (static) on Ubuntu'
categories:
  - linux
  - program
  - tooltips
tags:
  - ffmpeg
  - static-build
---
### Ref
- https://trac.ffmpeg.org/wiki/CompilationGuide/Ubuntu

```bash
sudo apt purge ffmpeg libavutil* libswscale* libswresample* libavcodec* libavformat* libavdevice* libavfilter*
sudo apt autoremove

sudo apt-get update -qq && sudo apt-get -y install \
  autoconf \
  automake \
  build-essential \
  cmake \
  git-core \
  libunistring-dev \
  libass-dev \
  libfreetype6-dev \
  libgnutls28-dev \
  libsdl2-dev \
  libtool \
  libva-dev \
  libvdpau-dev \
  libvorbis-dev \
  libxcb1-dev \
  libxcb-shm0-dev \
  libxcb-xfixes0-dev \
  meson \
  ninja-build \
  pkg-config \
  texinfo \
  wget \
  yasm \
  zlib1g-dev
```

## STATIC BUILD

```bash
export FF_SOURCES=/mnt/data/opensource/ffmpeg/ffmpeg_sources
export FF_BUILD=/mnt/data/opensource/ffmpeg/ffmpeg_build
export FF_INSTALL=/mnt/data/opensource/ffmpeg/ffmpeg_install
export NUM_CORES=8
```

#### === VIDEO

```bash
# NASM
An assembler used by some libraries.

If your repository provides nasm version ≥ 2.13 then you can install that instead of compiling:

sudo apt-get install nasm

Otherwise you can compile:

cd ${FF_SOURCES} && \
wget https://www.nasm.us/pub/nasm/releasebuilds/2.15.05/nasm-2.15.05.tar.bz2 && \
tar xjvf nasm-2.15.05.tar.bz2 && \
cd nasm-2.15.05 && \
./autogen.sh && \
PATH="${FF_INSTALL}/bin:$PATH" ./configure --prefix="${FF_BUILD}" --bindir="${FF_INSTALL}/bin" && \
make -j8 && \
make -j8 install


# libx264
H.264 video encoder. See the H.264 Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-gpl --enable-libx264.

If your repository provides libx264-dev version ≥ 118 then you can install that instead of compiling:

sudo apt-get install libx264-dev

Otherwise you can compile:

cd ${FF_SOURCES} && \
git -C x264 pull 2> /dev/null || git clone --depth 1 https://code.videolan.org/videolan/x264.git && \
cd x264 && \
PATH="${FF_INSTALL}/bin:$PATH" PKG_CONFIG_PATH="${FF_BUILD}/lib/pkgconfig" ./configure --prefix="${FF_BUILD}" --bindir="${FF_INSTALL}/bin" --enable-static --enable-pic && \
PATH="${FF_INSTALL}/bin:$PATH" \
make -j8 && \
make -j8 install


# libx265
H.265/HEVC video encoder. See the H.265 Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-gpl --enable-libx265.

If your repository provides libx265-dev version ≥ 68 then you can install that instead of compiling:

sudo apt-get install libx265-dev libnuma-dev

Warning: Currently and unlike other libraries, you have to get the full libx265 repository (so remove --depth 1 argument in git clone). Indeed, it will be longer, but it is necessary to allow generating x265.pc file, which is needed to build ffmpeg with --enable-libx265. Without this, ffmpeg build will be broken.

If you cannot, or don't want to get the full libx265 repository, please use the version provided by apt instead.

Otherwise you can compile:

sudo apt-get install libnuma-dev && \
cd ${FF_SOURCES} && \
git -C x265_git pull 2> /dev/null || git clone https://bitbucket.org/multicoreware/x265_git && \
cd x265_git/build/linux && \
PATH="${FF_INSTALL}/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="${FF_BUILD}" -DENABLE_SHARED=off ../../source && \
PATH="${FF_INSTALL}/bin:$PATH" \
make -j8 && \
make -j8 install


# libvpx
VP8/VP9 video encoder/decoder. See the VP9 Video Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-libvpx.

If your repository provides libvpx-dev version ≥ 1.4.0 then you can install that instead of compiling:

sudo apt-get install libvpx-dev

Otherwise you can compile:

cd ${FF_SOURCES} && \
git -C libvpx pull 2> /dev/null || git clone --depth 1 https://chromium.googlesource.com/webm/libvpx.git && \
cd libvpx && \
PATH="${FF_INSTALL}/bin:$PATH" ./configure --prefix="${FF_BUILD}" --disable-examples --disable-unit-tests --enable-vp9-highbitdepth --as=yasm && \
PATH="${FF_INSTALL}/bin:$PATH" \
make -j8 && \
make -j8 install

```

#### === AUDIO

```bash

# libfdk-aac
AAC audio encoder. See the AAC Audio Encoding Guide for more information and usage examples.

Requires ffmpeg to be configured with --enable-libfdk-aac (and --enable-nonfree if you also included --enable-gpl).

If your repository provides libfdk-aac-dev then you can install that instead of compiling:

sudo apt-get install libfdk-aac-dev

Otherwise you can compile:

cd ${FF_SOURCES} && \
git -C fdk-aac pull 2> /dev/null || git clone --depth 1 https://github.com/mstorsjo/fdk-aac && \
cd fdk-aac && \
autoreconf -fiv && \
./configure --prefix="${FF_BUILD}" --disable-shared && \
make && \
make install


# libmp3lame
MP3 audio encoder.

Requires ffmpeg to be configured with --enable-libmp3lame.

If your repository provides libmp3lame-dev version ≥ 3.98.3 then you can install that instead of compiling:

sudo apt-get install libmp3lame-dev

Otherwise you can compile:

cd ${FF_SOURCES} && \
wget -O lame-3.100.tar.gz https://downloads.sourceforge.net/project/lame/lame/3.100/lame-3.100.tar.gz && \
tar xzvf lame-3.100.tar.gz && \
cd lame-3.100 && \
PATH="${FF_INSTALL}/bin:$PATH" ./configure --prefix="${FF_BUILD}" --bindir="${FF_INSTALL}/bin" --disable-shared --enable-nasm && \
PATH="${FF_INSTALL}/bin:$PATH" make && \
make install


# libopus
Opus audio decoder and encoder.

Requires ffmpeg to be configured with --enable-libopus.

If your repository provides libopus-dev version ≥ 1.1 then you can install that instead of compiling:

sudo apt-get install libopus-dev

Otherwise you can compile:

cd ${FF_SOURCES} && \
git -C opus pull 2> /dev/null || git clone --depth 1 https://github.com/xiph/opus.git && \
cd opus && \
./autogen.sh && \
./configure --prefix="${FF_BUILD}" --disable-shared && \
make && \
make install


# libaom
AV1 video encoder/decoder:

Warning: libaom does not yet appear to have a stable API, so compilation of libavcodec/libaomenc.c may occasionally fail. Just wait a day or two for us to catch up with these annoying changes, re-download ffmpeg-snapshot.tar.bz2, and try again. Or skip libaom altogether.

cd ${FF_SOURCES} && \
git -C aom pull 2> /dev/null || git clone --depth 1 https://aomedia.googlesource.com/aom && \
mkdir -p aom_build && \
cd aom_build && \
PATH="${FF_INSTALL}/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="${FF_BUILD}" -DENABLE_SHARED=off -DENABLE_NASM=on ../aom && \
PATH="${FF_INSTALL}/bin:$PATH" \
make -j8 && \
make -j8 install


# libsvtav1
AV1 video encoder/decoder. Only the encoder is supported by FFmpeg, so building of the decoder is disabled.

Requires ffmpeg to be configured with --enable-libsvtav1.

cd ${FF_SOURCES} && \
git -C SVT-AV1 pull 2> /dev/null || git clone https://gitlab.com/AOMediaCodec/SVT-AV1.git && \
mkdir -p SVT-AV1/build && \
cd SVT-AV1/build && \
PATH="${FF_INSTALL}/bin:$PATH" cmake -G "Unix Makefiles" -DCMAKE_INSTALL_PREFIX="${FF_BUILD}" -DCMAKE_BUILD_TYPE=Release -DBUILD_DEC=OFF -DBUILD_SHARED_LIBS=OFF .. && \
PATH="${FF_INSTALL}/bin:$PATH" \
make -j8 && \
make -j8 install


# libdav1d
AV1 decoder, much faster than the one provided by libaom.

Requires ffmpeg to be configured with --enable-libdav1d.

If your repository provides libdav1d-dev, you can install that instead of compiling:

sudo apt-get install libdav1d-dev

Otherwise you'll need can build from source. Users whose distributions don't provide a recent enough version of meson (0.49.0 or newer) will need to install a more up-to-date version. This is easily done via the Python Package Index:

sudo apt-get install python3-pip && \
pip3 install --user meson

NASM version 2.14 or newer is required for AVX-512 support. See the NASM section for how to install/build. Alternatively, disable AVX-512 in Meson setup with -Denable_avx512=false.

To compile:

sudo apt-get purge meson && \
sudo apt-get install python3-pip && \
pip3 install --upgrade meson && \
cd ${FF_SOURCES} && \
git -C dav1d pull 2> /dev/null || git clone --depth 1 https://code.videolan.org/videolan/dav1d.git && \
mkdir -p dav1d/build && \
cd dav1d/build && \
PATH="${FF_INSTALL}/bin:$PATH"; $HOME/.local/bin/meson setup -Denable_tools=false -Denable_tests=false --default-library=static .. --prefix "${FF_BUILD}" --libdir="${FF_BUILD}/lib" && \
ninja && \
ninja install


# libvmaf
Library for calculating the VMAF video quality metric. Requires ffmpeg to be configured with --enable-libvmaf. Currently an issue in libvmaf also requires FFmpeg to be built with --ld="g++" for a static build to succeed.

To compile:

sudo apt-get purge meson && \
sudo apt-get install python3-pip && \
pip3 install --upgrade meson && \
cd ${FF_SOURCES} && \
wget https://github.com/Netflix/vmaf/archive/v2.1.1.tar.gz -O vmaf_v2.1.1.tar.gz && \
tar xvf vmaf_v2.1.1.tar.gz && \
mkdir -p vmaf-2.1.1/libvmaf/build &&\
cd vmaf-2.1.1/libvmaf/build && \
PATH="${FF_INSTALL}/bin:$PATH"; $HOME/.local/bin/meson setup -Denable_tests=false -Denable_docs=false --buildtype=release --default-library=static .. --prefix "${FF_BUILD}" --bindir="${FF_BUILD}/bin" --libdir="${FF_BUILD}/lib" && \
ninja && \
ninja install

```

#### === FFMPEG (Source package vs git: https://stackoverflow.com/a/36904949)

```bash
cd ${FF_SOURCES} && \
wget -O ffmpeg-snapshot.tar.bz2 https://ffmpeg.org/releases/ffmpeg-snapshot.tar.bz2 && \
tar xjvf ffmpeg-snapshot.tar.bz2 && \
cd ffmpeg && \
PATH="${FF_INSTALL}/bin:$PATH" \
PKG_CONFIG_PATH="${FF_BUILD}/lib/pkgconfig" ./configure \
  --pkg-config-flags="--static" \
  --extra-cflags="-I${FF_BUILD}/include" \
  --extra-ldflags="-L${FF_BUILD}/lib" \
  --extra-libs="-lpthread -lm" \
  --ld="g++" \
  --prefix="${FF_INSTALL}" \
  --enable-gpl \
  --enable-gnutls \
  --enable-libfreetype \
  --enable-nonfree \
  --enable-libx264 \
  --enable-libx265 \
  --enable-libvpx \
  --enable-libaom \
  --enable-libsvtav1 \
  --enable-libdav1d \
&& PATH="${FF_INSTALL}/bin:$PATH" \
make -j8 && \
make -j8 install && \
hash -r

  --enable-libass \
  --enable-libfdk-aac \
  --enable-libmp3lame \
  --enable-libopus \
  --enable-libvorbis \

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
