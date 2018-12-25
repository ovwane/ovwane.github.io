---
title: TP-LINK 703 固件更新 OpenWrt LEDE 17.01.6
date: 2018-12-25 08:06:52
tags:
---

## 下载OpenWrt固件 

[Index of /snapshots/trunk/ar71xx/generic/](http://archive.openwrt.org/snapshots/trunk/ar71xx/generic/)

>openwrt的固件一般分两种类型：factory原厂固件、sysupgrade固件

> factory多了一些验证的东西，用于在原厂固件的基础上进行升级。

> 普通家用路由一般不是openwrt固件，如果要将家用路由升级为openwrt固件，就可以用factory刷到路由上。sysupgrade是在openwrt路由基础上升级固件，无论你是原厂固件或者本身就是openwrt固件，要升级到openwrt，factory都适用，但是sysupgrade只能用在升级，TTL救砖的时候就不能用sysupgrade。sysupgrade不包含数据分区，factory带，factory预留原厂分区，sysupgrade只包含openwrt分区。

> 有一个公式:sysupgrade.bin+空闲空间+系统的配置空间=factory.bin的大小

> 在openwrt wiki中有专门描述sysupgrade：

> sysupgrade替换linux内核和squash文件系统，擦除整个jffs2部分。能保留配置文件，但不能保留二进制安装文件。

> 描述了几种sysupgrade方法，但没有描述在web界面的更新，也没有描述factory和sysupgrade的区别。

sha256sums

```shell
http://downloads.openwrt.org/releases/17.01.6/targets/ar71xx/generic/

http://downloads.openwrt.org/releases/17.01.6/targets/ar71xx/generic/sha256sums
```

官方固件刷机

```shell
wget http://downloads.openwrt.org/releases/17.01.6/targets/ar71xx/generic/lede-17.01.6-ar71xx-generic-tl-wr703n-v1-squashfs-factory.bin
```

## OpenWrt 升级固件

下载固件

```shell
http://downloads.openwrt.org/releases/17.01.6/targets/ar71xx/generic/lede-17.01.6-ar71xx-generic-tl-wr703n-v1-squashfs-sysupgrade.bin
```

登录OpenWrt

```shell
telnet 192.168.1.1
```

执行命令，下载固件到路由器上

```shell
cd /tmp

wget http://192.168.1.225:8000/openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-sysup
grade.bin
```

刷新系统

```verilog
root@OpenWrt:/tmp# cat /proc/mtd
dev:    size   erasesize  name
mtd0: 00020000 00010000 "u-boot"
mtd1: 0011f5dc 00010000 "kernel"
mtd2: 002b0a24 00010000 "rootfs"
mtd3: 00050000 00010000 "rootfs_data"
mtd4: 00010000 00010000 "art"
mtd5: 003d0000 00010000 "firmware"
```

写入固件到mtd5

```shell
mtd -r write openwrt-ar71xx-generic-tl-wr703n-v1-squashfs-sysupgrade.bin firmware
```

## OpenWrt 恢复出厂设置

登录

```shell
ssh root@192.168.1.1
```

```shell
firstboot
```

日志

```verilog
root@OpenWrt:~# firstboot
This will erase all settings and remove any installed packages. Are you sure? [N/y]
y
/dev/mtdblock3 is mounted as /overlay, only erasing files
```

```shell
mtd -r erase rootfs_data
```

## 初始化配置

设置网络

```verilog
root@LEDE:~# cat /etc/config/network

config interface 'loopback'
	option ifname 'lo'
	option proto 'static'
	option ipaddr '127.0.0.1'
	option netmask '255.0.0.0'

config globals 'globals'
	option ula_prefix 'fd5f:def1:6b18::/48'

config interface 'lan'
	option type 'bridge'
	option ifname 'eth0'
	option proto 'static'
	option ipaddr '192.168.1.1'
	option netmask '255.255.255.0'
	option ip6assign '60'
```

修改/etc/config/network文件，首先修改lan接口配置，注释掉此行：

vim /etc/config/network

```
# option ifname 'eth0'
```

然后增加wan接口，如果你上级网络是DHCP的，则文件的末尾添加：

```
config interface 'wan'
    option ifname 'eth0'
    option proto 'dhcp'
```

最后结果

```verilog
config interface 'wan'
        option ifname 'eth0'
        option proto 'dhcp'

config interface 'lan'
        option type 'bridge'
        # option ifname 'eth0'
        option proto 'static'
        option ipaddr '10.8.7.1'
        option netmask '255.255.255.0'
        option ip6assign '60'
```

开启无线网

```verilog
root@LEDE:~# cat /etc/config/wireless

config wifi-device 'radio0'
	option type 'mac80211'
	option channel '11'
	option hwmode '11g'
	option path 'platform/ar933x_wmac'
	option htmode 'HT20'
	option disabled '1'

config wifi-iface 'default_radio0'
	option device 'radio0'
	option network 'lan'
	option mode 'ap'
	option ssid 'LEDE'
	option encryption 'none'
```

修改`/etc/config/wireless`文件，注释掉

vim /etc/config/wireless

```
# option disabled 1
```

# u盘启动openwrt(含u盘挂载)

```shell
opkg remove ip6tables
opkg remove odhcp6c

```

更新软件列表（每次重启路由器后，需要先运行一次这个，才能安装软件包）

 ```shell
opkg update
 ```

1.安装移动存储设备支持

```shell
opkg install kmod-usb-storage  
```

```verilog
root@LEDE:~# opkg install kmod-usb-storage
Installing kmod-usb-storage (4.4.153-1) to root...
Downloading http://downloads.lede-project.org/releases/17.01.6/targets/ar71xx/generic/packages/kmod-usb-storage_4.4.153-1_mips_24kc.ipk
Installing kmod-scsi-core (4.4.153-1) to root...
Downloading http://downloads.lede-project.org/releases/17.01.6/targets/ar71xx/generic/packages/kmod-scsi-core_4.4.153-1_mips_24kc.ipk
Configuring kmod-scsi-core.
Configuring kmod-usb-storage.
root@LEDE:~#
# 大概用了60kb
```

立刻就可以查看u盘及其分区

```shell
ls /dev
```

观察里面是否出现sda sda1 sda2 sda3等字样

2.安装EXT4文件系统

```shell
opkg install kmod-fs-ext4
```

```verilog
# 大概用了221kb
```



6.安装开机从u盘启动

```shell
opkg install block-mount
```

```shell
mkdir -p /tmp/introot
mkdir -p /tmp/extroot
mount --bind / /tmp/introot
mount /dev/sda1 /tmp/extroot
tar -C /tmp/introot -cvf - . | tar -C /tmp/extroot -xf -
umount /tmp/introot
umount /tmp/extroot
```

```verilog
root@LEDE:~# opkg install block-mount
Installing block-mount (2018-04-16-6609e98a-1) to root...
Downloading http://downloads.lede-project.org/releases/17.01.6/targets/ar71xx/generic/packages/block-mount_2018-04-16-6609e98a-1_mips_24kc.ipk
Configuring block-mount.
this file has been obsoleted. please call "/sbin/block mount" directly
root@LEDE:~#
# 大概用了36kb
```

LEDE 17.01.6

```verilog
root@LEDE:~# df -h
Filesystem                Size      Used Available Use% Mounted on
/dev/root                 2.3M      2.3M         0 100% /rom
tmpfs                    13.7M     60.0K     13.7M   0% /tmp
tmpfs                    13.7M     48.0K     13.7M   0% /tmp/root
tmpfs                   512.0K         0    512.0K   0% /dev
/dev/mtdblock3          384.0K    212.0K    172.0K  55% /overlay
overlayfs:/overlay      384.0K    212.0K    172.0K  55% /
root@LEDE:~# cat /proc/version
Linux version 4.4.153 (buildbot@builds-02.infra.lede-project.org) (gcc version 5.4.0 (LEDE GCC 5.4.0 r3101-bce140e) ) #0 Thu Aug 30 12:10:54 2018
root@LEDE:~# cat /etc/openwrt_release
DISTRIB_ID='LEDE'
DISTRIB_RELEASE='17.01.6'
DISTRIB_REVISION='r3979-2252731af4'
DISTRIB_CODENAME='reboot'
DISTRIB_TARGET='ar71xx/generic'
DISTRIB_ARCH='mips_24kc'
DISTRIB_DESCRIPTION='LEDE Reboot 17.01.6 r3979-2252731af4'
DISTRIB_TAINTS=''
root@LEDE:~# opkg list-installed
base-files - 173.6-r3979-2252731af4
busybox - 1.25.1-4
dnsmasq - 2.78-6
dropbear - 2017.75-5
firewall - 2017-05-27-a4d98aea-1
fstools - 2018-04-16-6609e98a-1
fwtool - 1
hostapd-common - 2016-12-19-ad02e79d-7
ip6tables - 1.4.21-3
iptables - 1.4.21-3
iw - 4.9-1
iwinfo - 2016-09-21-fd9e17be-1
jshn - 2018-01-07-1dafcd78-1
jsonfilter - 2016-07-02-dea067ad-1
kernel - 4.4.153-1-33d452ad71ac13bc6dc71df37efa5ec7
kmod-ath - 4.4.153+2017-01-31-14
kmod-ath9k - 4.4.153+2017-01-31-14
kmod-ath9k-common - 4.4.153+2017-01-31-14
kmod-cfg80211 - 4.4.153+2017-01-31-14
kmod-gpio-button-hotplug - 4.4.153-2
kmod-ip6tables - 4.4.153-1
kmod-ipt-conntrack - 4.4.153-1
kmod-ipt-core - 4.4.153-1
kmod-ipt-nat - 4.4.153-1
kmod-lib-crc-ccitt - 4.4.153-1
kmod-mac80211 - 4.4.153+2017-01-31-14
kmod-nf-conntrack - 4.4.153-1
kmod-nf-conntrack6 - 4.4.153-1
kmod-nf-ipt - 4.4.153-1
kmod-nf-ipt6 - 4.4.153-1
kmod-nf-nat - 4.4.153-1
kmod-nls-base - 4.4.153-1
kmod-ppp - 4.4.153-1
kmod-pppoe - 4.4.153-1
kmod-pppox - 4.4.153-1
kmod-slhc - 4.4.153-1
kmod-usb-core - 4.4.153-1
kmod-usb2 - 4.4.153-1
lede-keyring - 2017-01-20-a50b7529-1
libblobmsg-json - 2018-01-07-1dafcd78-1
libc - 1.1.16-1
libgcc - 5.4.0-1
libip4tc - 1.4.21-3
libip6tc - 1.4.21-3
libiwinfo - 2016-09-21-fd9e17be-1
libiwinfo-lua - 2016-09-21-fd9e17be-1
libjson-c - 0.12.1-1
libjson-script - 2018-01-07-1dafcd78-1
liblua - 5.1.5-1
libnl-tiny - 0.1-5
libpthread - 1.1.16-1
libubox - 2018-01-07-1dafcd78-1
libubus - 2017-02-18-34c6e818-1
libubus-lua - 2017-02-18-34c6e818-1
libuci - 2018-01-01-141b64ef-1
libuci-lua - 2018-01-01-141b64ef-1
libuclient - 2018-08-03-ae1c656f-1
libxtables - 1.4.21-3
logd - 2017-03-10-16f7e161-1
lua - 5.1.5-1
luci - git-18.201.27126-7bf0367-1
luci-app-firewall - git-18.201.27126-7bf0367-1
luci-base - git-18.201.27126-7bf0367-1
luci-lib-ip - git-18.201.27126-7bf0367-1
luci-lib-jsonc - git-18.201.27126-7bf0367-1
luci-lib-nixio - git-18.201.27126-7bf0367-1
luci-mod-admin-full - git-18.201.27126-7bf0367-1
luci-proto-ipv6 - git-18.201.27126-7bf0367-1
luci-proto-ppp - git-18.201.27126-7bf0367-1
luci-theme-bootstrap - git-18.201.27126-7bf0367-1
mtd - 23.1
netifd - 2017-01-25-650758b1-1
odhcp6c - 2017-01-30-c13b6a05-2
odhcpd - 2018-05-27-59339a76-4
opkg - 2017-12-08-9f61f7ac-1
ppp - 2.4.7-12
ppp-mod-pppoe - 2.4.7-12
procd - 2018-01-22-9a4036fb-1
rpcd - 2018-05-13-82062195-1
swconfig - 11
uboot-envtools - 2015.10-1
ubox - 2017-03-10-16f7e161-1
ubus - 2017-02-18-34c6e818-1
ubusd - 2017-02-18-34c6e818-1
uci - 2018-01-01-141b64ef-1
uclient-fetch - 2018-08-03-ae1c656f-1
uhttpd - 2017-11-04-a235636a-1
uhttpd-mod-ubus - 2017-11-04-a235636a-1
usign - 2015-07-04-ef641914-1
wpad-mini - 2016-12-19-ad02e79d-7
root@LEDE:~#
```

OpenWrt 显示系统版本和GCC的版本

```shell
cat /proc/version

uname -a

cat /etc/openwrt_release
```

## 参考

[找到国外牛人给出的不通过TTL刷openwrt到1.7版本的703n的方法](https://www.right.com.cn/forum/thread-159078-1-1.html)

[openwrt的sysupgrade和factory固件的区别](https://www.cnblogs.com/simonid/p/6368111.html)

[TP-Link 703N刷OpenWrt的实践](https://blog.csdn.net/ydogg/article/details/8945766)

[openwrt 恢 复 出厂设置](https://blog.csdn.net/winux123/article/details/51973134)

[Openwrt 重置所有设置](http://see.sl088.com/wiki/Openwrt_%E9%87%8D%E7%BD%AE%E6%89%80%E6%9C%89%E8%AE%BE%E7%BD%AE)

[TL-WR703Nv1.7刷写openwrt固件](https://www.jianshu.com/p/4479207efcce)

[Openwrt学习笔记（二）——Flash Layout and file system](https://blog.csdn.net/lee244868149/article/details/57076615)

[Openwrt 启动过程](https://www.cnblogs.com/fastwave2004/articles/4251625.html)

[OPKG命令执行过程分析](http://www.cnblogs.com/zhangbaoqiang/p/4792628.html)

[openwrt 查看相应的硬件信息](https://blog.csdn.net/mcusun2000/article/details/51130434)

未用

[tplink tl-wr703n v1.7最新固件 免ttl刷openwrt刷breed](https://www.right.com.cn/forum/forum.php?mod=viewthread&tid=184971)