---
date: 2017-05-08 13:20
---

## 网络设置

```shell
vim /Library/Preferences/VMware\ Fusion/networking
```

answer VNET_1_HOSTONLY_NETMASK 255.255.255.0
answer VNET_1_HOSTONLY_SUBNET 192.168.2.0
answer VNET_8_HOSTONLY_NETMASK 255.255.255.0
answer VNET_8_HOSTONLY_SUBNET 192.168.5.0



### 安装 macOS 系统

下载 `安装 macOS Mojave.app`



## 问题

### 找不到可以连接的有效对等进程

> 关闭 Virtualbox

```shell
/Library/Application\ Support/VirtualBox/LaunchDaemons/VirtualBoxStartup.sh stop
```



## 参考

[给VMWare Fusion设置固定IP](http://www.up4dev.com/2016/10/15/vmware-fusion-static-ip/)