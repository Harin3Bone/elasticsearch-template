# Elasticsearch
<img alt="Elasticsearch" src="https://img.shields.io/badge/Elasticsearch-005571?style=flat&logo=Elasticsearch&logoColor=FFFFFF">&nbsp;
<img alt="Logstash" src="https://img.shields.io/badge/Logstash-005571?style=flat&logo=Logstash&logoColor=FFFFFF">&nbsp;
<img alt="Kibana" src="https://img.shields.io/badge/Kibana-005571?style=flat&logo=Kibana&logoColor=FFFFFF">&nbsp;
<img alt="Docker" src="https://img.shields.io/badge/Docker-2496ED?&style=flat&logo=docker&logoColor=ffffff">&nbsp;
<!-- <img alt="Beats" src="https://img.shields.io/badge/Beats-005571?style=flat&logo=Beats&logoColor=FFFFFF">&nbsp; -->

## Description
This repository made for build simple of ELK Stack with Docker.

## Prerequisite
* [Docker](https://docs.docker.com/engine/install/ubuntu/)
* [Docker Compose](https://docs.docker.com/compose/install/)

<!-- ## Quick Start

## Default Value
| Variable Name | Default value | Datattype | Description |
|:--------------|:--------------|:---------:|------------:|
|A|B|C|D|

Create `.env` file to define your own value -->

## Setup
**Step 1:** Download `elasticsearch.yml` from elastic github [here](https://github.com/elastic/elasticsearch/blob/master/distribution/src/config/elasticsearch.yml)

> ## Note 
> `elasticsearch.yml` is config file for Elasticsearch use for advance configuration later. Recommend to add into container

**Step 2:** Download `kibana.yml` from elastic github [here](https://github.com/elastic/kibana/blob/main/config/kibana.yml)

> ## Note 
> `kibana.yml` is config file for Elasticsearch use for advance configuration later. Recommend to add into container

**Step 3:** Add `Elasticsearch` node into `docker-compose.yml`
```yaml
version: "3.8"

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.15.2
    container_name: elasticsearch
    environment:
      - node.name=elasticsearc
      - cluster.name=es-docker-cluster
      - cluster.initial_master_nodes=elasticsearch
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - elastic_vol:/usr/share/elasticsearch/data
      - elastic_data:/var/lib/elasticsearch
      - elastic_log:/var/log/elasticsearch
      - ./elasticsearch.yml:/etc/elasticsearch/elasticsearch.yml
    ports:
      - 9200:9200
    networks:
      - elastic_net
```

**Step 4:** Add `Kibana` node into `docker-compose.yml`
```yaml
  kibana:
    image: docker.elastic.co/kibana/kibana:7.15.2
    container_name: kibana
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    volumes:
      - kibana_vol:/usr/share/kibana
      - ./kibana.yml:/etc/kibana/kibana.yml
    ports:
      - 5601:5601
    networks:
      - elastic_net
    depends_on:
      - elasticsearch
```

**Step 5:** Add `Logstash` node into `docker-compose.yml`
```yaml
  logstash:
    image: docker.elastic.co/logstash/logstash:7.15.1
    container_name: logstash
    volumes:
      - logstash_vol:/usr/share/logstash/config
      - logstash_log:/var/log/logstash
    networks:
      - elastic_net
```

**Step 6:** Add volume into `docker-compose.yml`
```yaml
volumes:
  elastic_vol: {}
  elastic_data: {}
  elastic_log: {}
  kibana_vol: {}
  logstash_vol: {}
  logstash_log: {}
```

**Step 7:** Add network into `docker-compose.yml`
```yaml
networks:
  elastic_net:
    external: false
    driver: bridge
```

Then `docker-compose.yml` will look like this
```yaml
version: "3.8"

services:
  elasticsearch:
    image: docker.elastic.co/elasticsearch/elasticsearch:7.15.2
    container_name: elasticsearch
    environment:
      - node.name=elasticsearc
      - cluster.name=es-docker-cluster
      - cluster.initial_master_nodes=elasticsearch
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms512m -Xmx512m"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - elastic_vol:/usr/share/elasticsearch/data
      - elastic_data:/var/lib/elasticsearch
      - elastic_log:/var/log/elasticsearch
      - ./elasticsearch.yml:/etc/elasticsearch/elasticsearch.yml
    ports:
      - 9200:9200
    networks:
      - elastic_net

  kibana:
    image: docker.elastic.co/kibana/kibana:7.15.2
    container_name: kibana
    environment:
      - ELASTICSEARCH_HOSTS=http://elasticsearch:9200
    volumes:
      - kibana_vol:/usr/share/kibana
      - ./kibana.yml:/etc/kibana/kibana.yml
    ports:
      - 5601:5601
    networks:
      - elastic_net
    depends_on:
      - elasticsearch
      
  logstash:
    image: docker.elastic.co/logstash/logstash:7.15.1
    container_name: logstash
    volumes:
      - logstash_vol:/usr/share/logstash/config
      - logstash_log:/var/log/logstash
    networks:
      - elastic_net


volumes:
  elastic_vol: {}
  elastic_data: {}
  elastic_log: {}
  kibana_vol: {}
  logstash_vol: {}
  logstash_log: {}

networks:
  elastic_net:
    external: false
    driver: bridge
```

**Step 8:** Start server
```bash
docker-compose up -d
```

## Reference
- [Elasticsearch.yml](https://github.com/elastic/elasticsearch/blob/master/distribution/src/config/elasticsearch.yml)
- [Kibana.yml](https://github.com/elastic/kibana/blob/main/config/kibana.yml)

## Contributor
<a href="https://github.com/Harin3Bone"><img src="https://img.shields.io/badge/Harin3Bone-181717?style=flat&logo=github&logoColor=ffffff"></a>