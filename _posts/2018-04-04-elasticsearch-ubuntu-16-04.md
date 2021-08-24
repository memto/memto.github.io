---
layout: post
ads: true
comments: true
published: true
title: Elasticsearch Ubuntu 16.04
categories:
  - linux
  - tooltips
tags:
  - elasticsearch
---
## Ref:
	https://www.elastic.co/guide/en/elasticsearch/reference/5.0/deb.html
    https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-elasticsearch-on-ubuntu-16-04
    
#### Download and install elasticsearch debian package manually
	download deb file: https://www.elastic.co/downloads/elasticsearch
	$ dpkg -i elasticsearch-<Version>.deb
    
#### Download jdk and setup JAVA_HOME
	download jdk: http://www.oracle.com/technetwork/java/javase/downloads/index.html
    (ex: jdk-10_linux-x64_bin.tar.gz for now)
    extract the downloaded tar.gz (ex: /home/dvn0zz/jdk-10)

#### set JAVA_HOME (error: could not find java; set JAVA_HOME or ensure java is in PATH)
	elasticsearch cannot get JAVA_HOME export from .bashrc or .profile
    it needs to be set in /etc/default/elasticsearch (ex: JAVA_HOME=/home/dvn0zz/jdk-10)
    
#### Manage as systemd service
	sudo /bin/systemctl enable elasticsearch.service
    sudo systemctl start elasticsearch.service
	sudo systemctl stop elasticsearch.service
    sudo systemctl status elasticsearch.service
    sudo journalctl --unit elasticsearch --since  "2016-10-30 18:17:16"
    
#### Verify
	curl -X GET 'http://localhost:9200'
