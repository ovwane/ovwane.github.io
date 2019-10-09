---
title: Ubuntu 18.04 使用教程
date: 2018-12-07 11:39:16
tags:
---

## 配置IP地址

位置 /etc/netplan/*.yaml

```yaml
network:
    ethernets:
        enp0s5:
            addresses:
            - 10.8.8.118/24
            dhcp4: no
            gateway4: 10.8.8.1
            nameservers:
                addresses:
                - 114.114.114.114
            optional: true
    version: 2
```

配置生效

```shell
netplan apply
```

查看DNS

```shell
systemd-resolve --status|grep 'DNS Servers'
```



## 安装

SSH

```shell
apt install -y openssh-server
```



Docker

```
snap install docker
```





## 参考

[如何在 Ubuntu 18.04 下正确配置网络](https://www.hi-linux.com/posts/49513.html)