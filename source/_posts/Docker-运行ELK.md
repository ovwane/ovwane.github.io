---
title: Docker 运行ELK
date: 2018-10-19 15:52:38
tags:
---

```shell
docker pull sebp/elk:642
```

运行

```shell
docker run -d \
--name elk-624 \
-e LOGSTASH_START=0 \
-e ES_HEAP_SIZE="1g" \
--network=sonar_sonarnet \
-p 5601:5601 -p 9200:9200 -p 5044:5044 \
sebp/elk:642
```

