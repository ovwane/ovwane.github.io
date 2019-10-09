---
title: Raspbian Stretch 初始化
date: 2017-09-07 11:17:18
---

# Raspbian Stretch 初始化
```
RASPBIAN STRETCH LITE
Minimal image based on Debian Stretch
Version:August 2017
Release date:2017-08-16
Kernel version:4.9
Release notes:Link
```
## 默认用户名是pi 密码是raspberry

#### [默认没有开启ssh](http://downloads.raspberrypi.org/raspbian/release_notes.txt)
```
2016-11-25:
  * SSH disabled by default; can be enabled by creating a file with name "ssh" in boot partition
  * Prompt for password change at boot when SSH enabled with default password unchanged
```

```
在SD卡boot分区新建一个文件
touch ssh
```
sudo -s

```
df -h

root@raspberrypi:/home/pi# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        15G  1.1G   13G   8% /
devtmpfs        460M     0  460M   0% /dev
tmpfs           464M     0  464M   0% /dev/shm
tmpfs           464M  6.2M  458M   2% /run
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           464M     0  464M   0% /sys/fs/cgroup
/dev/mmcblk0p1   42M   21M   21M  51% /boot
tmpfs            93M     0   93M   0% /run/user/1000
```
#### raspi-config
```
# 扩展系统分区
7.Advanced Options->A1 Expand Filesystem

# 更改时区
4.Localisation Options->I2 Change Timezone Aisa/Shanghai

# 重启系统
reboot
```
```
df -h

root@raspberrypi:/home/pi# df -h
Filesystem      Size  Used Avail Use% Mounted on
/dev/root        15G  1.1G   13G   8% /
devtmpfs        460M     0  460M   0% /dev
tmpfs           464M     0  464M   0% /dev/shm
tmpfs           464M   12M  452M   3% /run
tmpfs           5.0M  4.0K  5.0M   1% /run/lock
tmpfs           464M     0  464M   0% /sys/fs/cgroup
/dev/mmcblk0p1   42M   21M   21M  51% /boot
tmpfs            93M     0   93M   0% /run/user/1000
```

#### [设置静态IP地址](https://gaomf.cn/2016/10/27/Raspberry_Pi_Static_IP/)
```
vi /etc/dhcpcd.conf

interface enxb827eb15f13c
static ip_address=192.168.3.254/24
static routers=192.168.3.1
static domain_name_servers=114.114.114.114 8.8.8.8

# 重启系统
reboot
```

#### 更新软件源
##### [USTC](https://lug.ustc.edu.cn/wiki/mirrors/help/raspbian)
```
cat >/etc/apt/sources.list<<EOF
deb http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main non-free contrib
deb-src http://mirrors.ustc.edu.cn/raspbian/raspbian/ stretch main non-free contrib
EOF

# 更新系统
apt-get update && apt-get upgrade
```

#### 常用软件
```
apt-get -y install lrzsz vim git unzip 
```