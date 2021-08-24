---
layout: post
ads: true
comments: true
published: false
title: 'Postgresql on Ubuntu '
---
#### Install

```bash
curl https://www.pgadmin.org/static/packages_pgadmin_org.pub | sudo apt-key add
sudo sh -c 'echo "deb https://ftp.postgresql.org/pub/pgadmin/pgadmin4/apt/$(lsb_release -cs) pgadmin4 main" > /etc/apt/sources.list.d/pgadmin4.list
sudo apt update
```

```bash
# https://www.postgresql.org/download/linux/ubuntu/

# core database server
sudo apt install postgresql
# additional supplied modules (part of the postgresql-xx package in version 10 and later) 
sudo apt install postgresql-contrib
# pgAdmin 4 graphical administration utility
sudo apt install pgadmin4 
# end
```
