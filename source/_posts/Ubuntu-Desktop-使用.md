---
title: Ubuntu Desktop 使用
date: 2018-12-07 11:39:16
tags:
---

> Ubuntu 20.04 LTS



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

修改 [清华 apt 源](https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/)

> https://developer.aliyun.com/mirror/ubuntu

SSH

```shell
apt install -y openssh-server
```



常用工具

```
apt install -y vim lrzsz git
```



Android Studio

```
snap install android-studio --classic
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



### VNC Server

```bash
apt install tigervnc-standalone-server
```

```
vncserver -localhost no

vncserver -kill :*

vncserver --list
```



## 安装 Vagrant

> https://www.myfreax.com/how-to-install-vagrant-on-ubuntu-18-04/

```
apt install -y virtualbox vagrant 
```

> 使用 libvirt 可以把 virtualbox 替换为：`apt install -y qemu-kvm libvirt-clients libvirt-daemon-system bridge-utils virt-manager`
>
> https://bugs.launchpad.net/ubuntu/+source/vagrant-libvirt/+bug/1771430
>
> Virtualbox 扩展：https://download.virtualbox.org/virtualbox/6.1.10/Oracle_VM_VirtualBox_Extension_Pack-6.1.10.vbox-extpack
>
> `VBoxManage extpack install ./Oracle_VM_VirtualBox_Extension_Pack-6.1.10.vbox-extpack`
>
> https://www.zcfy.cc/article/how-to-install-and-use-vboxmanage-on-ubuntu-16-04-and-use-its-command-line-options



```
apt -y install virtualbox virtualbox-ext-pack vagrant
```



## 参考

[如何在 Ubuntu 18.04 下正确配置网络](https://www.hi-linux.com/posts/49513.html)