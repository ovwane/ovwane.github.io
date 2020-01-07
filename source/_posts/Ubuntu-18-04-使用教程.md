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

修改为 [清华 apt 源](https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/)

SSH

```shell
apt install -y openssh-server
```



Docker

```
snap install docker
```



```
apt install -y vim lrzsz git
```



### 安装 Docker

如果你过去安装过 docker，先删掉

```
apt remove docker docker-engine docker.io
```



安装依赖

```
apt install -y apt-transport-https ca-certificates curl gnupg2 software-properties-common
```



信任 Docker 的 GPG 公钥

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```



添加软件仓库

```
add-apt-repository "deb [arch=amd64] https://mirrors.tuna.tsinghua.edu.cn/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
```



安装

```
apt update
apt install -y docker-ce
```



开机自启动

```
systemctl enable docker.service
systemctl start docker.service
```



安装 docker-compose



## 参考

[如何在 Ubuntu 18.04 下正确配置网络](https://www.hi-linux.com/posts/49513.html)