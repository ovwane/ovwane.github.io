---
title: CentOS 升级系统
date: 2020-05-14 08:52:06
tags:
---

# CentOS 

CentOS 6.10 升级到 CentOS 7



<!--more-->

添加源

```bash
cat <<EOF > /etc/yum.repos.d/upgrade.repo
[upgrade]
name=upgrade
baseurl=http://buildlogs.centos.org/centos/6/upg/x86_64/
enabled=1
gpgcheck=0
EOF
```



安装工具

```bash
yum install -y redhat-upgrade-tool preupgrade-assistant preupgrade-assistant-contents
```



```bash
yum remove -y openscap && yum install -y http://buildlogs.centos.org/centos/6/upg/x86_64/Packages/openscap-1.0.8-1.0.1.el6.centos.x86_64.rpm
```



检查系统

```bash
preupg -l

preupg CentOS6_7
```



导入 Key

```bash
rpm --import https://mirrors.tuna.tsinghua.edu.cn/centos-vault/7.2.1511/os/x86_64/RPM-GPG-KEY-CentOS-7
```

> rpm -q gpg-pubkey
>
> rpm -e --allmatches gpg-pubkey-16ca1a56-4a100959



网络升级

```bash
centos-upgrade-tool-cli --network 7 --instrepo=https://mirrors.tuna.tsinghua.edu.cn/centos-vault/7.2.1511/os/x86_64/
```



iso 文件方式升级

```bash
wget https://mirrors.tuna.tsinghua.edu.cn/centos-vault/7.2.1511/isos/x86_64/CentOS-7-x86_64-DVD-1511.iso
```

```
centos-upgrade-tool-cli --iso=CentOS-7-x86_64-DVD-1511.iso --cleanup-post
```



## 参考

 [CentOS 6升级到CentOS 7 - 掘金](https://juejin.im/post/5d2167985188251d00042f4b) 

 [How to upgrade CentOS 6 to CentOS 7 - Knowledgebase - Owned-Networks](https://www.owned-networks.net/client_area/knowledgebase/41/How-to-upgrade-CentOS-6-to-CentOS-7.html) 

 [CentOS 6升级至Centos7 - larry-peng - 博客园](https://www.cnblogs.com/larrypeng/p/11539597.html) 

