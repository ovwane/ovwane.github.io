---
title: Docker 配置
date: 2019-01-17 10:08:22
tags:
---

vim /etc/docker/daemon.json

```
{
    "registry-mirrors": ["https://mqxz7mjm.mirror.aliyuncs.com"]
}
```

```shell
systemctl restart docker
```

