---
layout: post
ads: true
comments: true
published: true
title: Booting FreeBSD QEMU image (Ubuntu Host)
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

**Src**: ftp://ftp.freebsd.org/pub/FreeBSD/releases/VM-IMAGES/

```bash
$ wget ftp://ftp.freebsd.org/pub/FreeBSD/releases/VM-IMAGES/10.4-RELEASE/amd64/Latest/FreeBSD-10.4-RELEASE-amd64.qcow2.xz
$ xz -d FreeBSD-10.4-RELEASE-amd64.qcow2.xz
```

#### Booting
```bash
$ qemu-system-x86_64 --enable-kvm FreeBSD-10.4-RELEASE-amd64.qcow2
```

#### Login and create user, post-install setup
On first boot, login with account root (don't need password) then create new user there.

**Note**: **wheel** group can execute commands with superuser privileges. If you want to create a normal (unprivileged) user, you may leave this blank.

```bash
$ adduser

Username: XXX
Full name: YYY
Uid (Leave empty for default):
Login group [XXX]:
Login group is XXX. Invite XXX into other groups? []: wheel
Login class [default]:
Shell (sh csh tcsh nologin) [sh]:
Home directory [/home/XXX]:
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

#### Install package

**Note**: _pkg_ is package management tool for FreeBSD but it is not installed by default. simply type pkg on terminal and install it. also sudo need to be installed by hand.

```bash
$ pkg
$ pkg install sudo
```

#### Add user to sudoers

```bash
$ visudo
# under User privilege specification add bellow line (replace XXX by username want to be added)
XXX ALL=(ALL) ALL
```

#### Login as new user (XXX)

```bash
# press ctrl-D to exit current logged in session and back to login terminal
# enter username and password
login: XXX
Password: <your_password>
```

#### Escape from QEMU

Ref: [How do I get my mouse back from qemu/kvm?](https://unix.stackexchange.com/a/107634)

```bash
Ctrl + Left_Alt
```

#### SSH from host to qemu
- Ref:
	- [How to SSH from host to guest using QEMU?](https://unix.stackexchange.com/a/124777)
- Redirect port XXXX from host to port 22 on qemu

```bash
$ qemu-system-x86_64 --enable-kvm FreeBSD-11.2-RELEASE-amd64.qcow2 -redir tcp:XXXX::22
# if above is not work
$ qemu-system-x86_64 --enable-kvm FreeBSD-11.2-RELEASE-amd64.qcow2 -net user,hostfwd=tcp::XXXX-:22 -net nic
```

- Enable ssh on freebsd

```bash
$ service sshd status
# If the service is not running, add the following line to /etc/rc.conf
# sshd_enable="YES"
$ sudo service sshd start
```

- Connect from host to qemu

```bash
$ ssh <vmuser>@localhost -p <XXXX>
```