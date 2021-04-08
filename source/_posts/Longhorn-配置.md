---
title: Longhorn 配置
date: 2021-03-30 14:26:33
tags:
---

#  [Longhorn](https://longhorn.io/) 

Cloud native distributed block storage for Kubernetes

<!--more-->



## 安装

依赖

> https://longhorn.io/docs/1.1.0/deploy/install/

```shell
yum install iscsi-initiator-utils nfs-utils
```

>nfs-utils 是为了支持 RWX Volume。



### Helm 安装

> https://github.com/longhorn/charts

```shell
helm repo add longhorn https://charts.longhorn.io && helm repo update
```

安装

```shell
helm install longhorn \
--namespace longhorn-system \
--set defaultSettings.defaultDataPath="/data/longhorn/" \
--set defaultSettings.defaultReplicaCount=2 \
--set service.ui.type=NodePort \
--set service.ui.nodePort=30083 \
longhorn/longhorn
```



