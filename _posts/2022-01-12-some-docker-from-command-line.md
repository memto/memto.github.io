---
layout: post
ads: true
comments: true
published: false
title: Some docker from command line
---
## Postgresql

```bash
sudo docker run -d --restart always \
   --name app_postgres_dev \
   -p 9432:5432 \
   -e POSTGRES_PASSWORD=<YOUR_PASS> \
   -e PGDATA=/var/lib/postgresql/data/pgdata \
   -v <YOUR_PATH>/data/postgresql:/var/lib/postgresql/data \
    postgres:12
```
