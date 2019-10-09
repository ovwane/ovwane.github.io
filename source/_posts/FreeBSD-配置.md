---
title: FreeBSD 配置
date: 2017-08-24 12:04:33
---

Config em0|eth0
cd /etc/
cp rc.conf ~/
vi rc.conf
ifconfig_em0="inet 10.0.0.1 netmask 255.255.255.0"
defaultrouter="10.0.0.1"

Config DNS
cd /etc/
cp resolv.conf resolv.conf
vi resolv.conf
nameserver 8.8.8.8
nameserver 114.114.114.114

Restart netif and routing
/etc/rc.d/netif restart
/etc/rc.d/routing restart

Check IP
ifconfig


Change local sources server

Packages|pkg or Ports
Install pkg system|Getting Started with pkg|https://www.freebsd.org/doc/handbook/pkgng-intro.html
/usr/sbin/pkg

pkg update && pkg upgrade
pkg install vim



adduser su to root
pw usermod your_user -G wheel

test if this works:
groups your_user
