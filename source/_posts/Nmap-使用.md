---
title: nc 使用
date: 2021-12-21 15:41:41
tags:
---

# [Nmap](https://nmap.org/)



<!--more-->



扫描网段

```bash
nmap -sP 10.1.1.0/24
```

>--exclude 排除某个IP



扫描端口

```bash
nmap -p 443,22,80 10.1.1.1
```



扫描多个IP的端口

```bash
nmap -p 443,22,80 10.1.1.1-10
```

