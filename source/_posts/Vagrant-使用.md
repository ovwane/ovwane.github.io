---
title: Vagrant 使用
date: 2018-07-30 10:02:23
---

# Vagrant



## 安装

```
brew cask install vagrant
brew cask install virtualbox
```



**ubuntu 镜像下载地址**

[Ubuntu Cloud Images 18.04 LTS Daily Build](https://cloud-images.ubuntu.com/bionic/)

```shell
vagrant box add bionic-server-cloudimg-amd64-vagrant.box --name ubuntu/bionic64
```

**Vagrantfile**

```vagrantfile
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
end
```



## 管理

### 挂起

将当前的虚拟机挂起，以便下次恢复。

```
vagrant suspend
```



### 恢复

恢复虚拟机的上次状态。

```
vagrant resume
```

注意：我们每次挂起虚拟机后再重新启动它们的时候，看到的虚拟机中的时间依然是挂载时候的时间，这样将导致监控查看起来比较麻烦。因此请考虑先停机再重新启动虚拟机。



### 重启

清理虚拟机。

```
vagrant destroy
rm -rf .vagrant
```

### 

## 应用

### [本地分布式开发环境搭建（使用Vagrant和Virtualbox）](https://jimmysong.io/kubernetes-handbook/develop/using-vagrant-and-virtualbox-for-development.html)

> https://github.com/ovwane/kubernetes-vagrant-centos-cluster.git

```
git clone https://github.com/ovwane/kubernetes-vagrant-centos-cluster.git
```



## 参考

[ubuntu](https://app.vagrantup.com/ubuntu)/[bionic64](https://app.vagrantup.com/ubuntu/boxes/bionic64)