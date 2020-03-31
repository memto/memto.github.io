---
layout: post
ads: true
comments: true
published: false
title: Booting FreeBSD QEMU image
categories:
  - linux
  - tooltips
tags:
  - qemu-kvm
  - freebsd
---
#### Ref
- [qemu-kvm introduction](http://alexander.holbreich.org/qemu-kvm-introduction/)
- [How To Use QEMU To Test Operating Systems & Distributions](https://fosspost.org/tutorials/use-qemu-test-operating-systems-distributions)
- [Setting up QEMU-KVM for kernel development](https://www.collabora.com/news-and-blog/blog/2017/01/16/setting-up-qemu-kvm-for-kernel-development/)

#### Install QEMU (qemu-kvm)
```bash
$ sudo apt install qemu-kvm
```

#### Download FreeBSD qcow2 image (FreeBSD-10.4-RELEASE-amd64.qcow2.xz)

Src: ftp://ftp.freebsd.org/pub/FreeBSD/releases/VM-IMAGES/

```bash
$ wget ftp://ftp.freebsd.org/pub/FreeBSD/releases/VM-IMAGES/10.4-RELEASE/amd64/Latest/FreeBSD-10.4-RELEASE-amd64.qcow2.xz
```

#### Extract (using right click and extract, tar does not work)

#### Booting
```bash
$ qemu-system-x86_64 --enable-kvm FreeBSD-10.4-RELEASE-amd64.qcow2
```

#### Login and create user
On first boot, login with account root (don't need password) then create new user there.

**Note**: **wheel** group can execute commands with superuser privileges. If you want to create a normal (unprivileged) user, you may leave this blank.

```bash
$ sudo adduser

Username: XXX
Full name: XXX
Uid (Leave empty for default):
Login group [sammy]:
Login group is sammy. Invite sammy into other groups? []: wheel
Login class [default]:
Shell (sh csh tcsh nologin) [sh]:
Home directory [/home/sammy]:
Home directory permissions (Leave empty for default):
Use password-based authentication? [yes]:
Use an empty password? (yes/no) [no]:
Use a random password? (yes/no) [no]:
Enter password: password
Enter password again: password
Lock out the account after creation? [no]:
```

#### View directory layout
```bash
$ man hier
```
