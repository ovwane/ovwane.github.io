---
title: Vagrant Ubuntu 配置
date: 2018-07-30 10:02:23
---

# Vagrant Ubuntu 配置

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

## 参考

[ubuntu](https://app.vagrantup.com/ubuntu)/[bionic64](https://app.vagrantup.com/ubuntu/boxes/bionic64)