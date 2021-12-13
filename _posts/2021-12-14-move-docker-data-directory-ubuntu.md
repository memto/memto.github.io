---
layout: post
ads: true
comments: true
published: false
title: Move docker data directory Ubuntu
categories:
  - linux
  - program
tags:
  - docker
  - docker-root
---

### Move docker data directory Ubuntu

```bash
#== current Dir
$ sudo docker info | grep -i root
 Docker Root Dir: /var/lib/docker

#== Stop service
$ sudo service docker stop

#== add config
$ sudo vim /etc/docker/daemon.json
{ 
   "data-root": "/path/to/your/docker" 
}

$ sudo rsync -aP /var/lib/docker/ /path/to/your/docker
$ sudo mv /var/lib/docker /var/lib/docker.old

#== start service and verify
$ sudo service docker start
$ sudo docker info | grep -i root
 Docker Root Dir: /path/to/your/docker

$ sudo rm -rf /var/lib/docker.old

```