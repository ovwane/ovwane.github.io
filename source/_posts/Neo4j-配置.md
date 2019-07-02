---
title: Neo4j 配置
date: 2019-06-13 07:56:27
tags:
---



## 使用 Docker 运行 Neo4j

[docker-neo4j](https://github.com/neo4j/docker-neo4j)

[Neo4j Docker](https://hub.docker.com/_/neo4j)



运行

```shell
docker run -d --name=neo4j-3.5.6 --publish=7474:7474 --publish=7687:7687 --volume=$HOME/docker/neo4j/data:/data --env=NEO4J_AUTH=none neo4j:3.5.6
```

>--env=NEO4J_AUTH=none 关闭密码认证



访问

[http://127.0.0.1:7474/browser/](http://127.0.0.1:7474/browser/)

用户名：`neo4j`，密码：`neo4j`



## [Neo4j Browser](https://github.com/neo4j/neo4j-browser)

新建图

> [neo4j的搭建和实例使用](https://blog.csdn.net/u010086122/article/details/82390945)

```
create(suxun:Person_{name:"苏洵",age:80,sex:"男"})
create(sushi:Person_{name:"苏轼",age:53,sex:"男"})
create(suzhe:Person_{name:"苏辙",age:51,sex:"男"})
create(suxiaomei:Person_{name:"苏小妹",age:45,sex:"女"})
create(susun:Person_{name:"苏孙",age:29,sex:"男"})
create(suxiaosun:Person_{name:"苏重孙",age:6,sex:"女"})
```

```
match(p1:Person_),(p2:Person_)
where p1.name="苏洵" and p2.name = "苏轼"
create (p1) -[parent:Parent{relation:"父亲"}]-> (p2);

match(p1:Person_),(p2:Person_)
where p1.name="苏洵" and p2.name = "苏辙"
create (p1) -[parent:Parent{relation:"父亲"}] -> (p2);
 
match(p1:Person_),(p2:Person_)
where p1.name="苏洵" and p2.name = "苏小妹"
create (p1) -[parent:Parent{relation:"父亲"}] -> (p2);
 
match(p1:Person_),(p2:Person_)
where p1.name="苏轼" and p2.name = "苏孙"
create (p1) -[parent:Parent{relation:"父亲"}] -> (p2);
 
match(p1:Person_{name:"苏孙"}),(p2:Person_{name:"苏重孙"})
create (p1) -[parent:Parent{relation:"父亲"}] -> (p2);
```



查询渲染图

```
match(a)-[r:Parent]->(b) return a, r, b
```



```
:play https://guides.neo4j.com/sandbox/twitter-trolls/index.html
```



## Graph Visualization Tools

[Graph Visualization Tools](https://neo4j.com/developer/tools-graph-visualization/#_howto_graph_visualization_step_by_step)

[Graph Visualization With Neo4j Using Neovis.js](https://medium.com/neo4j/graph-visualization-with-neo4j-using-neovis-js-a2ecaaa7c379)

[Neovis](https://github.com/neo4j-contrib/neovis.js)

[Neo4j Sandbox](https://neo4j.com/sandbox-v2/)

[Russian Twitter Trolls](https://guides.neo4j.com/sandbox/twitter-trolls/index.html)

[d3.js可视化neo4j图数据库项目](https://blog.csdn.net/qq_34414916/article/details/81168275)

[Popoto.js](https://github.com/Nhogs/popoto)

[Data Visualization](https://medium.com/neo4j/tagged/data-visualization)