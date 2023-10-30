---
title: Docker 명령어
date: 2022-02-27 12:45:00 +0300
categories: [Docker]
tags: [Docker]
---

```shell
$ docker pull docker.elastic.co/elasticsearch/elasticsearch:8.0.0
```

```shell
$ docker run -d -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" --name elasticsearch7 docker.elastic.co/elasticsearch/elasticsearch:8.0.0
```

```shell
docker ps
```