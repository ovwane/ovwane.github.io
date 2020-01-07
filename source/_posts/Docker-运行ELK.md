---
title: Docker 运行ELK
date: 2018-10-19 15:52:38
tags:
---

# Elastic Stack

[Elastic Stack and Product Documentation | Elastic](https://www.elastic.co/guide/index.html)



```shell
docker pull sebp/elk:642
```

运行

```shell
docker run -d \
--name elk-642 \
-e LOGSTASH_START=0 \
-e ES_HEAP_SIZE="1g" \
--network=elk \
-p 5601:5601 -p 9200:9200 -p 5044:5044 \
sebp/elk:642
```



## 单独安装

### 安装 elasticsearch

[Install Elasticsearch with Docker | Elasticsearch Reference [6.5] | Elastic](https://www.elastic.co/guide/en/elasticsearch/reference/6.5/docker.html)

```shell
docker pull elasticsearch:6.5.4
```



### 安装 [elasticsearch-head](https://github.com/mobz/elasticsearch-head) 扩展

```shell
docker pull mobz/elasticsearch-head:5
```



### 安装 kibana

[Running Kibana on Docker | Kibana User Guide [6.5] | Elastic](https://www.elastic.co/guide/en/kibana/6.5/docker.html)

```shell
docker pull kibana:6.5.4
```



### 安装 logstash

```shell
docker pull logstash:6.5.4
```



### 安装 filebeat

```shell
docker pull filebeat:6.5.4
```



### 安装 metricbeat

```shell
docker pull docker.elastic.co/beats/metricbeat:6.5.4
```



### docker-compose

docker-compose.yml

```yaml
version: '3'
services:
  es-master:
    depends_on:
      - es-head
    image: elasticsearch:6.5.4
    container_name: es-master
    environment:
      - "ES_JAVA_OPTS=-Xms1g -Xmx1g"
    ulimits:
      memlock:
        soft: -1
        hard: -1
    volumes:
      - ./conf/es-master.yml:/usr/share/elasticsearch/config/elasticsearch.yml:ro
      - ~/docker/elk/es-master/data:/usr/share/elasticsearch/data
      - ~/docker/elasticsearch/es-master/plugins:/usr/share/elasticsearch/plugins
    ports:
      - 9200:9200
      - "9300:9300"
    networks:
      - esnet
  es-head:
    image: mobz/elasticsearch-head:5
    container_name: es-head
    restart: always
    ports:
      - "9100:9100"
    networks:
      - esnet
  kibana:
    depends_on:
      - es-master
    image: kibana:6.5.4
    container_name: kibana
    volumes:
      - ./conf/kibana.yml:/usr/share/kibana/config/kibana.yml:ro
    ports:
      - "5601:5601"
    networks:
      - esnet

networks:
  esnet:
```



**文件配置**

```shell
mkdir conf
```



conf/es-master.yml

```yaml
cluster.name: es-cluster
node.name: es-master
node.master: true
node.data: true
bootstrap.memory_lock: true
# discovery.zen.minimum_master_nodes: 1
# discovery.zen.ping.unicast.hosts: ["es-master", "es-node1"]
# discovery.zen.ping.unicast.hosts: ["es-master"]
network.host: es-master
http.cors.enabled: true
http.cors.allow-origin: "*"
xpack.monitoring.collection.enabled: true
```



conf/kibana.yml

```yaml
server.name: kibana
server.host: "0.0.0.0"
elasticsearch.url: http://es-master:9200
# xpack.monitoring.ui.container.elasticsearch.enabled: true
```



**启动**

```shell
docker-compose up -d
```



 **Inspect status of cluster**

```
curl http://127.0.0.1:9200/_cat/health
```

http://localhost:9200/_cat/health?v

http://localhost:9200/_cat/health?format=json&bytes=b



## 安装插件

查看已安装的插件

```shell
elasticsearch-plugin list
```



### [IK Analysis Plugin](https://github.com/medcl/elasticsearch-analysis-ik) (作者 Medcl)

简介：大名鼎鼎的ik分词，都懂的！

```shell
elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.5.4/elasticsearch-analysis-ik-6.5.4.zip
```



**测试**

```shell
curl -XPUT http://localhost:9200/index
```

```shell
curl -XGET "http://localhost:9200/index/_analyze" -H 'Content-Type: application/json' -d'
{
   "text":"中华人民共和国MN","tokenizer": "ik_max_word"
}'
```



## 参考

[ElasticSearch安装(以Docker的方式）](https://blog.csdn.net/guanheng68/article/details/81710406)

[docker-compose安装elasticsearch集群](https://www.cnblogs.com/mxmbk/p/9969008.html)

[elasticsearch 6.3.2 集群配置](https://blog.csdn.net/wanglei_storage/article/details/82218940)

[Kibana + ElasticSearch](https://www.cnblogs.com/showtime813/p/5714603.html)

[ElasticSearch常用插件整理](https://blog.csdn.net/zyc88888/article/details/79019865)

[Elasticsearch-RTF](https://github.com/medcl/elasticsearch-rtf)

 [Running the Elastic Stack on Docker | Getting Started [7.5] | Elastic](https://www.elastic.co/guide/en/elastic-stack-get-started/current/get-started-docker.html#get-started-docker) 