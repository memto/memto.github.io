---
layout: post
ads: true
comments: true
published: true
title: linux update-alternatives
categories:
  - linux
  - program
  - tooltips
tags:
  - gcc-version
  - g++ version
  - update-alternatives
---
  sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.3 10
  sudo update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-4.4 20

  sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.3 10
  sudo update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-4.4 20

  sudo update-alternatives --install /usr/bin/cc cc /usr/bin/gcc 30
  sudo update-alternatives --set cc /usr/bin/gcc

  sudo update-alternatives --install /usr/bin/c++ c++ /usr/bin/g++ 30
  sudo update-alternatives --set c++ /usr/bin/g++lp.

  sudo update-alternatives --config gcc
  sudo update-alternatives --config g++
