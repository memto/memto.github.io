---
layout: post
ads: true
comments: true
published: false
title: Elasticsearch docker on Ubuntu
categories:
  - linux
  - program
  - tooltips
tags:
  - docker
  - elasticsearch
  - kibana
---
#### Ref
- https://www.elastic.co/guide/en/elasticsearch/reference/current/getting-started.html
- https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html
- https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html
- https://github.com/deviantony/docker-elk
- https://github.com/quoeamaster/bigdata_blogs/tree/master/es_docker_deploy
- https://learn.liferay.com/dxp/latest/en/using-search/installing-and-upgrading-a-search-engine/elasticsearch/installing-elasticsearch.html

#### Run docker (https://www.elastic.co/guide/en/elasticsearch/reference/current/docker.html)

![](https://i.imgur.com/29N8C4d.png)

```bash
docker network create elastic

# Elasticsearh
docker run --name es01-test --net elastic -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" docker.elastic.co/elasticsearch/elasticsearch:7.13.4
# To check Elasticsearch, curl -X GET "localhost:9200/?pretty"

# Kibana
docker run --name kib01-test --net elastic -p 5601:5601 -e "ELASTICSEARCH_HOSTS=http://es01-test:9200" docker.elastic.co/kibana/kibana:7.13.4
# To access Kibana, go to http://localhost:5601
```

#### Docker-compose from template
https://github.com/memto/elk-local

```bash
cd elk-local
# update ELASTIC_PASSWORD in docker-compose.yml and disable paid features
docker-compose up
```

#### Custom Docker-compose (https://docs.docker.com/compose/compose-file/)

- Install docker-compose (https://docs.docker.com/compose/install/#install-compose)

![](https://i.imgur.com/PKr2ywj.png)

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```

- **Note:** 
	- there should be no space in environment setting
    
- ek-compose-single.yaml 
```yaml
version: "3.8"
   
services:
  es01:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.13.4
    ports:
      - 9200:9200
      - 9300:9300
    environment:
      # Use single node discovery in order to disable production mode and avoid bootstrap checks.
      # see: https://www.elastic.co/guide/en/elasticsearch/reference/current/bootstrap-checks.html
      - discovery.type=single-node
      # - transport.host=localhost # Opt1
      # - http.host=0.0.0.0
      # - transport.tcp.port=9300
      # - http.port=9200
      # - "ES_JAVA_OPTS=-Xmx256m -Xms256m -Des.enforce.bootstrap.checks=true" # Opt2
      # - "ES_JAVA_OPTS=-Xmx256m -Xms256m"
      # - bootstrap.memory_lock=false
      # - xpack.security.enabled=false
      # - ELASTIC_PASSWORD:changeme
    # volumes:
    #   - es-data:/usr/share/elasticsearch/data
    networks:
      - elastic

  kiba01:
    image: docker.elastic.co/kibana/kibana:7.13.4
    ports:
      - 5601:5601
    environment:
      - SERVER_NAME=kibana
      - ELASTICSEARCH_URL=http://es01:9200
      - ELASTICSEARCH_HOSTS=http://es01:9200
    depends_on:
      - es01
    networks:
      - elastic

networks:
  elastic:
    driver: bridge

volumes:
  es-data:
```

```bash
docker-compose -f ek-compose-single.yaml up --remove-orphan
```

- Ports info

![](https://i.imgur.com/pEI98Fp.png)