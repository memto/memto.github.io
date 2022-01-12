---
layout: post
ads: true
comments: true
published: false
title: Some docker from command line
---
#### Postgresql

```bash
sudo docker run -d --restart always \
   --name pgdev-container \
   -p 9432:5432 \
   -e POSTGRES_PASSWORD=<YOUR_PASS> \
   -e PGDATA=/var/lib/postgresql/data/pgdata \
   -v <YOUR_PATH>/data/postgresql:/var/lib/postgresql/data \
    postgres:12
```

#### pgadmin

```bash
sudo docker run -d --restart always --network="host" \
    --name pgadmin-container \
    -e 'PGADMIN_DEFAULT_EMAIL=pagadmin@docker.net' \
    -e 'PGADMIN_DEFAULT_PASSWORD=<YOUR_PASS>' \
    dpage/pgadmin4
```
