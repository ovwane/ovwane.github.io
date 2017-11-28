title: KVM安装配置
date: 2017-08-31 11:08:00
categories:
- 技术
- Linux
tags:
- CentOS
- KVM
---
[KVM虚拟化学习笔记](http://koumm.blog.51cto.com/703525/1288795)[Centos 6.8安装配置KVM](http://www.cnblogs.com/rhjeans/p/5822190.html)[KVM 与 CentOS-6](https://wiki.centos.org/zh/HowTos/KVM)[KVM安装配置笔记](http://www.showerlee.com/archives/309)
# 简介
KVM 虚拟机的管理工具
准确来说，KVM 仅仅是 Linux 内核的一个模块。管理和创建完整的 KVM 虚拟机，需要更多的辅助工具。QEMU-KVM：在 Linux 系统中，首先我们可以用 modprobe 命令加载 KVM 模块，如果用 RPM 安装 KVM 软件包，系统会在启动时自动加载模块。加载了模块后，才能进一步通过其他工具创建虚拟机。但仅有 KVM 模块是远远不够的，因为用户无法直接控制内核模块去做事情，还必须有一个用户空间的工具。关于用户空间的工具，KVM 的开发者选择了已经成型的开源虚拟化软件 QEMU。QEMU 是一个强大的虚拟化软件，它可以虚拟不同的 CPU 构架。比如说在 x86 的 CPU 上虚拟一个 Power 的 CPU，并利用它编译出可运行在 Power 上的程序。KVM 使用了 QEMU 的基于 x86 的部分，并稍加改造，形成可控制 KVM 内核模块的用户空间工具 QEMU-KVM。所以 Linux 发行版中分为 内核部分的 KVM 内核模块和 QEMU-KVM 工具。这就是 KVM 和 QEMU 的关系。 Libvirt、virsh、virt-manager：尽管 QEMU-KVM 工具可以创建和管理 KVM 虚拟机，RedHat 为 KVM 开发了更多的辅助工具，比如 libvirt、libguestfs 等。原因是 QEMU 工具效率不高，不易于使用。Libvirt 是一套提供了多种语言接口的 API，为各种虚拟化工具提供一套方便、可靠的编程接口，不仅支持 KVM，而且支持 Xen 等其他虚拟机。使用 libvirt，你只需要通过 libvirt 提供的函数连接到 KVM 或 Xen 宿主机，便可以用同样的命令控制不同的虚拟机了。Libvirt 不仅提供了 API，还自带一套基于文本的管理虚拟机的命令 virsh，你可以通过使用 virsh 命令来使用 libvirt 的全部功能。但最终用户更渴望的是图形用户界面，这就是 virt-manager。他是一套用 python 编写的虚拟机管理图形界面，用户可以通过它直观地操作不同的虚拟机。Virt-manager 就是利用 libvirt 的 API 实现的。
 
# 配置
安装配置KVM 相关软件 
1. 系统要求：
***

半虚拟化是不能运行与安装KVM虚拟机的
***

处理器需求:需要一台可以运行最新linux内核的Intel处理器(含VT虚拟化技术)或AMD处理器(含SVM安全虚拟机技术的AMD处理器, 也叫AMD-V)。可以使用如下命令检查：

```
egrep '(vmx|svm)' --color=always /proc/cpuinfo
```
如果输出的结果包含 vmx，它是 Intel处理器虚拟机技术标志;如果包含 svm，它是 AMD处理器虚拟机技术标志;。如果你甚么都得不到，那应你的系统并没有支持虚拟化的处理 ，不能使用kvm。另外Linux 发行版本必须在64bit环境中才能使用KVM。

## 关闭selinux
## 关闭防火墙

# 安装
## 安装KVM模块管理工具
```
[root@hostname ~]#yum install -y libvirt-client #libvirt客户端，最主要的的功能之一就是在宿主机关机时通知虚拟机也关机。
[root@hostname ~]#yum install -y gpxe-roms-qemu #虚拟机iPXE启动固件，支持虚拟机网络启动。
[root@hostname ~]#yum install -y libvirt-python #libvirt为python提供的api。
[root@hostname ~]#yum install -y python-virtinst #一套python的虚拟机安装工具。
[root@hostname ~]#yum install -y qemu-kvm #kvm在用户控件运行的程序。
[root@hostname ~]#yum install -y virt-manager #基于libvirt的图像化虚拟机管理软件。
[root@hostname ~]#yum install -y libvirt #用于管理虚拟机，它提供了一套虚拟机操作api。
[root@hostname ~]#yum install -y virt-viewer #显示虚拟机控制台console
[root@hostname ~]#yum install -y virt-top #类似于top命令，查看虚拟机资源使用情况。
[root@hostname ~]#yum install -y virt-what #在虚拟机内部执行，查看虚拟机运行的虚拟化平台。
[root@hostname ~]#yum install -y qemu-img #用于操作虚拟机硬盘镜像的创建、查看和格式转换。
```
## 安装kvm软件包
```
yum -y install kvm python-virtinst libvirt tunctl bridge-utils virt-manager qemu-kvm-tools virt-viewer virt-v2v qemu-kvm qemu-img
```

## 安装kvm虚拟化一些管理工具包
```
yum -y install libguestfs-tools
```



## 确定正确加载kvm模块
运行命令 lsmod | grep kvm 检查 KVM 模块是否成功安装。如果结果类似于以下输出，那么 KVM 模块已成功安装：

```
[root@kvm ~]# lsmod | grep kvm
```

## 检查KVM是否成功安装
```
virsh -c qemu:///system list
```
#### 查看虚拟工具版本
```
virsh --version
0.10.2
```

### 手动配置虚拟网桥
#### 关闭networkmanager服务
```
chkconfig NetworkManager off
```

#### 创建br0网桥
```
cd  /etc/sysconfig/network-scripts/
cp ifcfg-eth0 ifcfg-br0
```
```
vi ifcfg-eth0

DEVICE="br0"
NM_CONTROLLED="no"
ONBOOT="yes"
BOOTPROTO=static
TYPE=Bridge
IPADDR=192.168.3.164
NETMASK=255.255.255.0
GATEWAY=192.168.3.1
```

#### 关闭了networkmanager服务之后，才能通过servicenetworkrestart管理网络
```
service network restart
service network status
```

#### 查看网桥br0
```
ifconfig
```

#### 查看网桥
```
brctl show
``` 

## linux kvm虚拟机安装
### 

#### 查看支持的操作系统
```
virt-install --os-variant=list | more
```

#### raw格式磁盘
```
virt-install --name=oeltest01 --ram 512 --vcpus=1 --disk path=/home/kvm/test02.img,size=7,bus=virtio --accelerate --cdrom /home/iso/debian-9.1.0-amd64-DVD-1.iso --vnc --vncport=5910 --vnclisten=0.0.0.0 --network bridge=br0,model=virtio --noautoconsole
```
#### qcow2格式(空间动态增长)
```
qemu-img create -f qcow2 /home/kvm/debian-1.img 10G
```
```
[root@it001 iso]# qemu-img create -f qcow2 /home/kvm/debian-1.img 10G
Formatting '/home/kvm/debian-1.img', fmt=qcow2 size=10737418240 encryption=off cluster_size=65536
```

```
virt-install --name=debian-1 --os-variant=debianwheezy --ram 1024 --vcpus=2 --disk path=/home/kvm/debian-1.img,format=qcow2,size=7,bus=virtio --accelerate --cdrom /home/iso/debian-9.1.0-amd64-DVD-1.iso --vnc --vncport=5910 --vnclisten=0.0.0.0 --network bridge=br0,model=virtio --noautoconsole
```
```
[root@it001 iso]# qemu-img create -f qcow2 /home/kvm/debian-1.img 10G
Formatting '/home/kvm/debian-1.img', fmt=qcow2 size=10737418240 encryption=off cluster_size=65536
[root@it001 iso]# virt-install --name=debian-1 --os-variant=debianwheezy --ram 1024 --vcpus=2 --disk path=/home/kvm/debian-1.img,format=qcow2,size=7,bus=virtio --accelerate --cdrom /home/iso/debian-9.1.0-amd64-DVD-1.iso--vnc --vncport=5910 --vnclisten=0.0.0.0 --network bridge=br0,model=virtio --noautoconsole

开始安装......
创建域......                                                                        |    0 B     00:00
域安装仍在进行。您可以重新连接
到控制台以便完成安装进程。
```

## kvm虚拟机日常管理与配置
### 查看KVM虚拟机配置文件及运行状态
#### KVM虚拟机默认配置文件位置
```
 /etc/libvirt/qemu/
autostart目录是配置kvm虚拟机开机自启动目录
```

#### virsh命令帮助
virsh help

#### 查看kvm虚拟机状态
virsh list --all

#### KVM虚拟机开机
virsh start debian-1

#### KVM虚拟机关机或断电
##### virsh关机
virsh shutdown debian-1

##### 强制关闭电源
virsh destroy debian-1

#### 通过配置文件启动虚拟机
virsh create /etc/libvirt/qemu/debian-1.xml

#### 配置开机自启动虚拟机
virsh autostart debian-1

#### 导出KVM虚拟机配置文件
virsh dumpxml debian-1 > /etc/libvirt/qemu/debian-2.xml

### 添加与删除KVM虚拟机
#### 删除kvm虚拟机
virsh undefine debian-1
***

该命令只是删除wintest01的配置文件，并不删除虚拟磁盘文件。如下图所示
***

#### 重新定义虚拟机配置文件
virsh define /etc/libvirt/qemu/debian-1.xml 

#### 编辑KVM虚拟机配置文件
virsh edit debian-1

### 其它virsh命令
#### virsh console 控制台管理linux虚拟机
virsh console debian-1

#### 挂起服务器
virsh suspend debian-1

#### 恢复服务器
virsh  resume debian-1

三、KVM默认网络配置
 
1、kvm上网有两种配置，
一种是default，它支持主机与虚拟机的互访，同时也支持虚拟机访问互联网，但不支持外界访问虚拟机。
另外一种方式是bridge方式，可以使用虚拟机成为网络中具有独立IP的主机。
 
默认的网络连接是virbr0，它的配置文件在/var/lib/libvirt/network目录下，默认配置为：

另外一种是网络桥接方式，配置如下：
配置eth0:

 配置:br0:
 
```
vi /etc/sysconfig/network-scripts/ifcfg-br0
--------------------
DEVICE="br0"
TYPE=Bridge
BOOTRPOTO=static
IPADDR=172.16.40.248
NETMASK=255.255.255.0
GATEWAY=172.16.40.254
ONBOOT=yes
----------------------
```
注:网桥模式需要在真机eth0配置文件中添加 BRIDGE="br0",否则真机与虚拟机无法互通.
   配置完毕后eth0口则不会显示地址信息,新配置的br0口会代替eth0口成为真机网口,装好的虚拟机eth0口将于真机br0口互通.
 
配置桥接网络之后，我们开始安装虚拟机
 
## 使用virt-manager建立一个KVM虚拟机
 
virt-manager 是基于 libvirt 的图像化虚拟机管理软件，请注意不同的发行版上 virt-manager 的版本可能不同，图形界面和操作方法也可能不同。本文使用了红帽6企业版的 virt-manager-0.8.4-8。创建KVM虚拟机最简单的方法是通过virt-manager接口。从控制台窗口启动这个工具，从root身份输入virt-manager命令，点击file菜单的"新建"选项virt-manager接口界面

接下来，出现的画面，大家都已经很熟悉了。
