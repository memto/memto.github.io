---
layout: post
ads: true
comments: true
published: true
title: Some docker from command line
categories:
  - linux
  - program
  - tooltips
tags:
  - docker
---
#### Postgresql

```bash
sudo docker run -d --restart always \
   --name postgres12-container \
   -p 9432:5432 \
   -e POSTGRES_PASSWORD=<YOUR_PASS> \
   -e PGDATA=/var/lib/postgresql/data/pgdata \
   -v <YOUR_PATH>/data/postgresql:/var/lib/postgresql/data \
    postgres:12
```

#### pgadmin

- **Note**: need `--network="host"` to use `localhost` when connecting to local postgres container as above (Other solutions: can create a docker network and specify the network for both; or create both in a docker-compose)

```bash
sudo docker run -d --restart always \
	--network="host" \
    --name pgadmin-container \
    -e 'PGADMIN_DEFAULT_EMAIL=pagadmin@pgadmin-container.com' \
    -e 'PGADMIN_DEFAULT_PASSWORD=<YOUR_PASS>' \
    dpage/pgadmin4
```

#### Cloundbeaver

```bash
sudo docker run -d --restart unless-stopped \
	--name cloudbeaver \
	--network="host" \
	# -p 8978:8978 \
	-v <YOUR_PATH>/cloudbeaver:/opt/cloudbeaver/workspace \
 	\
	dbeaver/cloudbeaver:latest
```