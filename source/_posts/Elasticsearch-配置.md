---
title: Elasticsearch 配置
date: 2020-07-06 15:17:47
tags:
---

<!--more-->



## 启动

docker-compose.yml

```yaml
version: '2.2'
services:
  cerebro:
    image: lmenezes/cerebro:0.9.4
    container_name: cerebro
    ports:
      - "9000:9000"
    command:
      - -Dhosts.0.host=http://elasticsearch:9200
    networks:
      - es7net
  kibana:
    image: kibana:7.13.1
    container_name: kibana
    environment:
      - I18N_LOCALE=zh-CN
      - XPACK_GRAPH_ENABLED=true
      - TIMELION_ENABLED=true
      - XPACK_MONITORING_COLLECTION_ENABLED="true"
    ports:
      - "5601:5601"
    networks:
      - es7net
  elasticsearch:
    image: elasticsearch:7.13.1
    container_name: es7_01
    environment:
      - cluster.name=es7
      - node.name=es7_01
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms3072m -Xmx3072m"
      - discovery.seed_hosts=es7_01,es7_02
      - cluster.initial_master_nodes=es7_01,es7_02
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - ./es7data1:/usr/share/elasticsearch/data
    ports:
      - 9200:9200
    networks:
      - es7net
  elasticsearch2:
    image: elasticsearch:7.13.1
    container_name: es7_02
    environment:
      - cluster.name=es7
      - node.name=es7_02
      - bootstrap.memory_lock=true
      - "ES_JAVA_OPTS=-Xms3072m -Xmx3072m"
      - discovery.seed_hosts=es7_01,es7_02
      - cluster.initial_master_nodes=es7_01,es7_02
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - ./es7data2:/usr/share/elasticsearch/data
    networks:
      - es7net

networks:
  es7net:
    driver: bridge
```



用户名密码访问

```
curl --user logstash:logstash  http://172.17.7.64:9200
```

