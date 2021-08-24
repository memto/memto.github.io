---
layout: post
ads: true
comments: true
published: false
title: Kafka and kafka-magic docker-compose on Ubuntu
categories:
  - linux
  - program
  - tooltips
tags:
  - kafka
  - kafka-gui
  - kafka-magic
---
# kafka-local
docker-compose with kafka and kafka-magic to run on local

### Ref
- https://www.kafkamagic.com/start/

### Run
- run docker

```bash
git clone https://github.com/memto/kafka-local.git
docker-compose up
```

- register cluster in kafka-magic UI

![launch-kafka-magic](https://raw.githubusercontent.com/memto/kafka-local/main/docs/launch.png)

![register-cluster](https://raw.githubusercontent.com/memto/kafka-local/main/docs/register-cluster.png)

![register-cluster-info](https://raw.githubusercontent.com/memto/kafka-local/main/docs/register-cluster-info.png)

![using](https://raw.githubusercontent.com/memto/kafka-local/main/docs/using.png)
