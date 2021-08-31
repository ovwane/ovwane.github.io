---
Title: kubectl 使用
date: 2021-03-08 16:07
---

查看deploy 

```
kubectl get deploy -n kube-system
```



删除deploy

```
kubectl delete deploy kube-dns -n kube-system
```



查看服务

```
kubectl get svc -n kube-system
```



删除服务

```
kubectl delete svc kube-dns -n kube-system
```



查看pod

```
kubectl get pod -n kube-system
```



查看描述信息

```
kubectl describe pod kube-dns-201876264-3c341 -n kube-system
```



