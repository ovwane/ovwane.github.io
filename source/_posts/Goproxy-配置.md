---
title: Goproxy 配置
date: 2019-01-30 12:31:55
tags:
---



```shell
docker run -d --restart=always --name goproxy -e OPTS="http -p 33080" -p 0.0.0.0:33080:33080 snail007/goproxy:latest
```

