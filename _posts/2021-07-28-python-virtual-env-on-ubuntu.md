---
layout: post
ads: true
comments: true
published: true
title: Python virtual env on Ubuntu
categories:
  - linux
  - program
  - tooltips
tags:
  - python
  - virtual-env
  - pycharm
---
### Python 3

```bash
sudo apt install python3-venv
cd <project_folder>
python3 -m venv venv
source venv/bin/activate
```


### Python virtual env with pycharm

- From existing virtual env that created as above

![](https://i.imgur.com/vdqMpr5.png)

![](https://i.imgur.com/u9fPYtQ.png)

- Create new virtual env

![](https://i.imgur.com/bwgFimq.png)

![](https://i.imgur.com/qx52sLd.png)

- Install new package in virtual env

![](https://i.imgur.com/flNZfBr.png)


### Issues
- error: invalid command 'bdist_wheel' (https://stackoverflow.com/a/44862371)

```bash
Had to install the wheel package. Everything was up to date but still giving the error.

pip install wheel

then

python setup.py bdist_wheel 
```


