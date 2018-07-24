---
title: CentOS7 开启BBR
date: 2018-07-23 08:30:43
---

# CentOS7 开启BBR

安装kernel

```shell
yum remove -y kernel-headers kernel-tools kernel-tools-libs

rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org

rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-3.el7.elrepo.noarch.rpm

yum -y install grub2

#kernel-ml-4.17.9-1.el7.elrepo.x86_64
yum --enablerepo=elrepo-kernel install -y kernel-ml kernel-ml-headers kernel-ml-devel 
yum --enablerepo=elrepo-kernel install -y kernel-ml-doc  kernel-ml-tools  kernel-ml-tools-libs perf python-perf

ls -l /boot/vmlinuz*
 
mkdir /boot/grub

grub2-mkconfig -o /boot/grub/grub.cfg

rpm -qa | grep kernel

awk -F\' '$1=="menuentry " {print $2}' /etc/grub2.cfg
#egrep ^menuentry /etc/grub2.cfg | cut -f 2 -d \'

shutdown -r now

yum remove -y kernel

shutdown -r now
```

开启BBR

```shell
cat >>/etc/sysctl.conf << EOF
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr
EOF
```

```shell
sysctl -p

sysctl net.ipv4.tcp_available_congestion_control

sysctl net.core.default_qdisc

lsmod | grep bbr
```

## 参考

[Linode CentOS7开启Google TCP-BBR优化算法](https://blog.linuxeye.cn/452.html)

[一键安装最新内核并开启 BBR 脚本](https://teddysun.com/489.html)