title: COBBLER无人值守安装
date: 2015-12-26 11:22:15
categories:
- 技术
- Linux
tags:
- CentOS
- Cobbler
---
[COBBLER无人值守安装](http://www.zyops.com/autoinstall-cobbler)

## Cobbler介绍
Cobbler是一个Linux服务器安装的服务，可以通过网络启动(PXE)的方式来快速安装、重装物理服务器和虚拟机，同时还可以管理DHCP，DNS等。

Cobbler可以使用命令行方式管理，也提供了基于Web的界面管理工具(cobbler-web)，还提供了API接口，可以方便二次开发使用。

Cobbler是较早前的kickstart的升级版，优点是比较容易配置，还自带web界面比较易于管理。

Cobbler内置了一个轻量级配置管理系统，但它也支持和其它配置管理系统集成，如Puppet，暂时不支持SaltStack。

### Cobbler集成的服务
- PXE服务支持
- DHCP服务管理
- DNS服务管理(可选bind,dnsmasq)
- 电源管理
- Kickstart服务支持
- YUM仓库管理
- TFTP(PXE启动时需要)
- Apache(提供kickstart的安装源，并提供定制化的kickstart配置)

## Cobbler安装配置
### 系统环境准备
```
[root@linux-node1 ~]# cat /etc/redhat-release
CentOS release 6.7 (Final)
[root@linux-node1 ~]# uname -r
2.6.32-573.el6.x86_64
[root@linux-node1 ~]# getenforce
Disabled
[root@linux-node1 ~]# /etc/init.d/iptables status
iptables: Firewall is not running.
[root@linux-node1 ~]# ifconfig eth0|awk -F "[ :]+" 'NR==2 {print $4}'
10.0.0.7
[root@linux-node1 ~]# hostname
linux-node1.example.com
# 配置阿里云的epel源
[root@linux-node1 ~]# wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-6.rep
```

### 安装Cobbler
```
[root@linux-node1 ~]# yum -y install cobbler cobbler-web dhcp tftp-server pykickstart httpd
[root@linux-node1 ~]# rpm -ql cobbler  # 查看安装的文件，下面列出部分。
/etc/cobbler                  # 配置文件目录
/etc/cobbler/settings         # cobbler主配置文件，这个文件是YAML格式，Cobbler是python写的程序。
/etc/cobbler/dhcp.template    # DHCP服务的配置模板
/etc/cobbler/tftpd.template   # tftp服务的配置模板
/etc/cobbler/rsync.template   # rsync服务的配置模板
/etc/cobbler/iso              # iso模板配置文件目录
/etc/cobbler/pxe              # pxe模板文件目录
/etc/cobbler/power            # 电源的配置文件目录
/etc/cobbler/users.conf       # Web服务授权配置文件
/etc/cobbler/users.digest     # 用于web访问的用户名密码配置文件
/etc/cobbler/dnsmasq.template # DNS服务的配置模板
/etc/cobbler/modules.conf     # Cobbler模块配置文件
/var/lib/cobbler              # Cobbler数据目录
/var/lib/cobbler/config       # 配置文件
/var/lib/cobbler/kickstarts   # 默认存放kickstart文件
/var/lib/cobbler/loaders      # 存放的各种引导程序
/var/www/cobbler              # 系统安装镜像目录
/var/www/cobbler/ks_mirror    # 导入的系统镜像列表
/var/www/cobbler/images       # 导入的系统镜像启动文件
/var/www/cobbler/repo_mirror  # yum源存储目录
/var/log/cobbler              # 日志目录
/var/log/cobbler/install.log  # 客户端系统安装日志
/var/log/cobbler/cobbler.log  # cobbler日志
```

### 配置Cobbler
```
[root@linux-node1 ~]# /etc/init.d/httpd restart
停止 httpd：                                               [失败]
正在启动 httpd：                                           [确定]
[root@linux-node1 ~]# /etc/init.d/cobblerd start
Starting cobbler daemon:                                   [确定]
[root@linux-node1 ~]# cobbler check  # 检查Cobbler的配置，如果看不到下面的结果，再次执行/etc/init.d/cobblerd restart
The following are potential configuration items that you may want to fix:
1 : The 'server' field in /etc/cobbler/settings must be set to something other than localhost, or kickstarting features will not work.  This should be a resolvable hostname or IP for the boot server as reachable by all machines that will use it.
2 : For PXE to be functional, the 'next_server' field in /etc/cobbler/settings must be set to something other than 127.0.0.1, and should match the IP of the boot server on the PXE network.
3 : some network boot-loaders are missing from /var/lib/cobbler/loaders, you may run 'cobbler get-loaders' to download them, or, if you only want to handle x86/x86_64 netbooting, you may ensure that you have installed a *recent* version of the syslinux package installed and can ignore this message entirely.  Files in this directory, should you want to support all architectures, should include pxelinux.0, menu.c32, elilo.efi, and yaboot. The 'cobbler get-loaders' command is the easiest way to resolve these requirements.
4 : change 'disable' to 'no' in /etc/xinetd.d/rsync
5 : debmirror package is not installed, it will be required to manage debian deployments and repositories
6 : The default password used by the sample templates for newly installed machines (default_password_crypted in /etc/cobbler/settings) is still set to 'cobbler' and should be changed, try: "openssl passwd -1 -salt 'random-phrase-here' 'your-password-here'" to generate new one
7 : fencing tools were not found, and are required to use the (optional) power management features. install cman or fence-agents to use them
Restart cobblerd and then run 'cobbler sync' to apply changes.
# 看着上面的结果，一个一个解决。
# 第1、2、6个问题，顺便修改其他功能
[root@linux-node1 ~]# cp /etc/cobbler/settings{,.ori}  # 备份
# server，Cobbler服务器的IP。
sed -i 's/server: 127.0.0.1/server: 10.0.0.7/' /etc/cobbler/settings
# next_server，如果用Cobbler管理DHCP，修改本项，作用不解释，看kickstart。
sed -i 's/next_server: 127.0.0.1/next_server: 10.0.0.7/' /etc/cobbler/settings
# 用Cobbler管理DHCP
sed -i 's/manage_dhcp: 0/manage_dhcp: 1/' /etc/cobbler/settings
# 防止循环装系统，适用于服务器第一启动项是PXE启动。
sed -i 's/pxe_just_once: 0/pxe_just_once: 1/' /etc/cobbler/settings
# 设置新装系统的默认root密码123456。下面的命令来源于提示6。random-phrase-here为干扰码，可以自行设定。
[root@linux-node1 ~]# openssl passwd -1 -salt 'oldboy' '123456'
$1$oldboy$Npg9Pt9k98Mlg0ZeqHAuN1
[root@linux-node1 ~]# vim /etc/cobbler/settings 
default_password_crypted: "$1$oldboy$Npg9Pt9k98Mlg0ZeqHAuN1" 
# 第3个问题
[root@linux-node1 ~]# cobbler get-loaders  # 会自动从官网下载
[root@linux-node1 ~]# cd /var/lib/cobbler/loaders/  # 下载的内容
[root@linux-node1 loaders]# ls
COPYING.elilo     COPYING.yaboot  grub-x86_64.efi  menu.c32    README
COPYING.syslinux  elilo-ia64.efi  grub-x86.efi     pxelinux.0  yaboot
# 第4个问题
[root@linux-node1 ~]# vim /etc/xinetd.d/rsync
disable = no
[root@linux-node1 ~]# /etc/init.d/xinetd restart
停止 xinetd：                                              [确定]
正在启动 xinetd：                                          [确定]
[root@linux-node1 ~]# /etc/init.d/cobblerd restart
Stopping cobbler daemon:                                   [确定]
Starting cobbler daemon:                                   [确定]
[root@linux-node1 ~]# cobbler check
The following are potential configuration items that you may want to fix:
1 : debmirror package is not installed, it will be required to manage debian deployments and repositories  # 和debian系统相关，不需要
2 : fencing tools were not found, and are required to use the (optional) power management features. install cman or fence-agents to use them # fence设备相关，不需要
Restart cobblerd and then run 'cobbler sync' to apply changes.
```

### 配置DHCP
```
# 修改cobbler的dhcp模版，不要直接修改dhcp本身的配置文件，因为cobbler会覆盖。
[root@linux-node1 ~]# vim /etc/cobbler/dhcp.template 
# 仅列出修改过的字段
……
subnet 10.0.0.0 netmask 255.255.255.0 {
     option routers             10.0.0.2;
     option domain-name-servers 10.0.0.2;
     option subnet-mask         255.255.255.0;
     range dynamic-bootp        10.0.0.100 10.0.0.200;
……
```

### 同步cobbler配置
```

# 同步最新cobbler配置，它会根据配置自动修改dhcp等服务。
[root@linux-node1 ~]# cobbler sync   # 同步所有配置，可以仔细看一下sync做了什么。
task started: 2015-12-03_204822_sync
task started (id=Sync, time=Thu Dec  3 20:48:22 2015)
running pre-sync triggers
cleaning trees
removing: /var/lib/tftpboot/pxelinux.cfg/default
removing: /var/lib/tftpboot/grub/images
copying bootloaders
trying hardlink /var/lib/cobbler/loaders/pxelinux.0 -> /var/lib/tftpboot/pxelinux.0
copying: /var/lib/cobbler/loaders/pxelinux.0 -> /var/lib/tftpboot/pxelinux.0
trying hardlink /var/lib/cobbler/loaders/menu.c32 -> /var/lib/tftpboot/menu.c32
trying hardlink /var/lib/cobbler/loaders/yaboot -> /var/lib/tftpboot/yaboot
trying hardlink /usr/share/syslinux/memdisk -> /var/lib/tftpboot/memdisk
trying hardlink /var/lib/cobbler/loaders/grub-x86.efi -> /var/lib/tftpboot/grub/grub-x86.efi
trying hardlink /var/lib/cobbler/loaders/grub-x86_64.efi -> /var/lib/tftpboot/grub/grub-x86_64.efi
copying distros to tftpboot
copying images
generating PXE configuration files
generating PXE menu structure
rendering DHCP files
generating /etc/dhcp/dhcpd.conf
rendering TFTPD files
generating /etc/xinetd.d/tftp
cleaning link caches
running post-sync triggers
running python triggers from /var/lib/cobbler/triggers/sync/post/*
running python trigger cobbler.modules.sync_post_restart_services
running: dhcpd -t -q
received on stdout: 
received on stderr: 
running: service dhcpd restart
received on stdout: 关闭 dhcpd：[确定]
正在启动 dhcpd：[确定]
received on stderr: 
running shell triggers from /var/lib/cobbler/triggers/sync/post/*
running python triggers from /var/lib/cobbler/triggers/change/*
running python trigger cobbler.modules.scm_track
running shell triggers from /var/lib/cobbler/triggers/change/*
*** TASK COMPLETE ***
# 再看一下dhcp的配置文件。
[root@linux-node1 ~]# less /etc/dhcp/dhcpd.conf
# ******************************************************************
# Cobbler managed dhcpd.conf file
# generated from cobbler dhcp.conf template (Thu Dec  3 12:48:23 2015)
# Do NOT make changes to /etc/dhcpd.conf. Instead, make your changes
# in /etc/cobbler/dhcp.template, as /etc/dhcpd.conf will be
# overwritten.
# ******************************************************************
ddns-update-style interim;
…………
```

### 开机启动
```
# 启动相关服务并设置开机启动(可选) 与第二种方法二选一
chkconfig httpd on
chkconfig xinetd on
chkconfig cobblerd on
chkconfig dhcpd on
/etc/init.d/httpd restart
/etc/init.d/xinetd restart
/etc/init.d/cobblerd restart
/etc/init.d/dhcpd restart
# 编写Cobbler相关服务启动脚本(可选)
cat >>/etc/init.d/cobbler<<EOF
#!/bin/bash
# chkconfig: 345 80 90
# description:cobbler
case \$1 in
  start)
    /etc/init.d/httpd start
    /etc/init.d/xinetd start
    /etc/init.d/dhcpd start
    /etc/init.d/cobblerd start
    ;;
  stop)
    /etc/init.d/httpd stop
    /etc/init.d/xinetd stop
    /etc/init.d/dhcpd stop
    /etc/init.d/cobblerd stop
    ;;
  restart)
    /etc/init.d/httpd restart
    /etc/init.d/xinetd restart
    /etc/init.d/dhcpd restart
    /etc/init.d/cobblerd restart
    ;;
  status)
    /etc/init.d/httpd status
    /etc/init.d/xinetd status
    /etc/init.d/dhcpd status
    /etc/init.d/cobblerd status
    ;;
  sync)
    cobbler sync
    ;;
  *)
    echo "Input error,please in put 'start|stop|restart|status|sync'!"
    exit 2
    ;;
esac
EOF
# chmod +x /etc/init.d/cobbler
# chkconfig cobbler on
```

## Cobbler的命令行管理
### 查看命令帮助
```
[root@linux-node1 ~]# cobbler
usage
=====
cobbler <distro|profile|system|repo|image|mgmtclass|package|file> ... 
        [add|edit|copy|getks*|list|remove|rename|report] [options|--help]
cobbler <aclsetup|buildiso|import|list|replicate|report|reposync|sync|validateks|version|signature|get-loaders|hardlink> [options|--help]
[root@linux-node1 ~]# cobbler import --help  # 导入镜像
Usage: cobbler [options]
Options:
  -h, --help            show this help message and exit
  --arch=ARCH           OS architecture being imported
  --breed=BREED         the breed being imported
  --os-version=OS_VERSION
                        the version being imported
  --path=PATH           local path or rsync location
  --name=NAME           name, ex 'RHEL-5'
  --available-as=AVAILABLE_AS
                        tree is here, don't mirror
  --kickstart=KICKSTART_FILE
                        assign this kickstart file
  --rsync-flags=RSYNC_FLAGS
                        pass additional flags to rsync
cobbler check    核对当前设置是否有问题
cobbler list     列出所有的cobbler元素
cobbler report   列出元素的详细信息
cobbler sync     同步配置到数据目录,更改配置最好都要执行下
cobbler reposync 同步yum仓库
cobbler distro   查看导入的发行版系统信息
cobbler system   查看添加的系统信息
cobbler profile  查看配置信息
```

### 导入镜像
```
[root@linux-node1 ~]# mount /dev/cdrom /mnt/  # 挂载CentOS7的系统镜像。
# 导入系统镜像
[root@linux-node1 ~]# cobbler import --path=/mnt/ --name=CentOS-7.1-x86_64 --arch=x86_64
# --path 镜像路径
# --name 为安装源定义一个名字
# --arch 指定安装源是32位、64位、ia64, 目前支持的选项有: x86│x86_64│ia64
# 安装源的唯一标示就是根据name参数来定义，本例导入成功后，安装源的唯一标示就是：CentOS-7.1-x86_64，如果重复，系统会提示导入失败。
[root@linux-node1 ~]# cobbler distro list  # 查看镜像列表
   CentOS-7.1-x86_64
# 镜像存放目录，cobbler会将镜像中的所有安装文件拷贝到本地一份，放在/var/www/cobbler/ks_mirror下的CentOS-7.1-x86_64目录下。因此/var/www/cobbler目录必须具有足够容纳安装文件的空间。
[root@linux-node1 ~]# cd /var/www/cobbler/ks_mirror/
[root@linux-node1 ks_mirror]# ls
CentOS-7.1-x86_64  config
[root@linux-node1 ks_mirror]# ls CentOS-7.1-x86_64/
CentOS_BuildTag  GPL       LiveOS    RPM-GPG-KEY-CentOS-7
EFI              images    Packages  RPM-GPG-KEY-CentOS-Testing-7
EULA             isolinux  repodata  TRANS.TBL
```

### 指定ks.cfg文件及调整内核参数
```
# Cobbler的ks.cfg文件存放位置
[root@linux-node1 ks_mirror]# cd /var/lib/cobbler/kickstarts/
[root@linux-node1 kickstarts]# ls  # 自带很多
default.ks    install_profiles  sample_autoyast.xml  sample_esxi4.ks  sample_old.seed
esxi4-ks.cfg  legacy.ks         sample_end.ks（默认使用的ks文件）        sample_esxi5.ks  sample.seed
esxi5-ks.cfg  pxerescue.ks      sample_esx4.ks       sample.ks
[root@linux-node1 kickstarts]# rz  # 上传准备好的ks文件
rz waiting to receive.
Starting zmodem transfer.  Press Ctrl+C to cancel.
Transferring Cobbler-CentOS-7.1-x86_64.cfg...
  100%       1 KB       1 KB/sec    00:00:01       0 Errors
[root@linux-node1 kickstarts]# mv Cobbler-CentOS-7.1-x86_64.cfg CentOS-7.1-x86_64.cfg
# 在第一次导入系统镜像后，Cobbler会给镜像指定一个默认的kickstart自动安装文件在/var/lib/cobbler/kickstarts下的sample_end.ks。
[root@linux-node1 ~]# cobbler list
distros:
   CentOS-7.1-x86_64
profiles:
   CentOS-7.1-x86_64
systems:
repos:
images:
mgmtclasses:
packages:
files:
# 查看安装镜像文件信息
[root@linux-node1 ~]# cobbler distro report --name=CentOS-7.1-x86_64
Name                           : CentOS-7.1-x86_64
Architecture                   : x86_64
TFTP Boot Files                : {}
Breed                          : redhat
Comment                        : 
Fetchable Files                : {}
Initrd                         : /var/www/cobbler/ks_mirror/CentOS-7.1-x86_64/images/pxeboot/initrd.img
Kernel                         : /var/www/cobbler/ks_mirror/CentOS-7.1-x86_64/images/pxeboot/vmlinuz
Kernel Options                 : {}
Kernel Options (Post Install)  : {}
Kickstart Metadata             : {'tree': 'http://@@http_server@@/cblr/links/CentOS-7.1-x86_64'}
Management Classes             : []
OS Version                     : rhel7
Owners                         : ['admin']
Red Hat Management Key         : <<inherit>>
Red Hat Management Server      : <<inherit>>
Template Files                 : {}
# 查看所有的profile设置
[root@linux-node1 ~]# cobbler profile report
# 查看指定的profile设置
[root@linux-node1 ~]# cobbler profile report --name=CentOS-7.1-x86_64
Name                           : CentOS-7.1-x86_64
TFTP Boot Files                : {}
Comment                        : 
DHCP Tag                       : default
Distribution                   : CentOS-7.1-x86_64
Enable gPXE?                   : 0
Enable PXE Menu?               : 1
Fetchable Files                : {}
Kernel Options                 : {}
Kernel Options (Post Install)  : {}
Kickstart                      : /var/lib/cobbler/kickstarts/sample_end.ks  -->默认ks文件
Kickstart Metadata             : {}
Management Classes             : []
Management Parameters          : <<inherit>>
Name Servers                   : []
Name Servers Search Path       : []
Owners                         : ['admin']
Parent Profile                 : 
Internal proxy                 : 
Red Hat Management Key         : <<inherit>>
Red Hat Management Server      : <<inherit>>
Repos                          : []
Server Override                : <<inherit>>
Template Files                 : {}
Virt Auto Boot                 : 1
Virt Bridge                    : xenbr0
Virt CPUs                      : 1
Virt Disk Driver Type          : raw
Virt File Size(GB)             : 5
Virt Path                      : 
Virt RAM (MB)                  : 512
Virt Type                      : kvm
# 编辑profile，修改关联的ks文件
[root@linux-node1 ~]# cobbler profile edit --name=CentOS-7.1-x86_64 --kickstart=/var/lib/cobbler/kickstarts/CentOS-7.1-x86_64.cfg
# 修改安装系统的内核参数，在CentOS7系统有一个地方变了，就是网卡名变成eno16777736这种形式，但是为了运维标准化，我们需要将它变成我们常用的eth0，因此使用下面的参数。但要注意是CentOS7才需要下面的步骤，CentOS6不需要。
[root@linux-node1 ~]# cobbler profile edit --name=CentOS-7.1-x86_64 --kopts='net.ifnames=0 biosdevname=0'
[root@linux-node1 ~]# cobbler profile report CentOS-7.1-x86_64
Name                           : CentOS-7.1-x86_64
TFTP Boot Files                : {}
Comment                        : 
DHCP Tag                       : default
Distribution                   : CentOS-7.1-x86_64
Enable gPXE?                   : 0
Enable PXE Menu?               : 1
Fetchable Files                : {}
Kernel Options                 : {'biosdevname': '0', 'net.ifnames': '0'}
Kernel Options (Post Install)  : {}
Kickstart                      : /var/lib/cobbler/kickstarts/CentOS-7.1-x86_64.cfg
Kickstart Metadata             : {}
Management Classes             : []
Management Parameters          : <<inherit>>
Name Servers                   : []
Name Servers Search Path       : []
Owners                         : ['admin']
Parent Profile                 : 
Internal proxy                 : 
Red Hat Management Key         : <<inherit>>
Red Hat Management Server      : <<inherit>>
Repos                          : []
Server Override                : <<inherit>>
Template Files                 : {}
Virt Auto Boot                 : 1
Virt Bridge                    : xenbr0
Virt CPUs                      : 1
Virt Disk Driver Type          : raw
Virt File Size(GB)             : 5
Virt Path                      : 
Virt RAM (MB)                  : 512
Virt Type                      : kvm
# 每次修改完都要同步一次
[root@linux-node1 ~]# cobbler sync
```

### 安装系统
可以很愉快的告诉你到这里就可以安装系统了！
有没有发现不美观的地方？
网址不是我的！改！

```
#修改Cobbler提示
[root@linux-node1 ~]# vim /etc/cobbler/pxe/pxedefault.template
MENU TITLE Cobbler | http://www.zyops.com
[root@linux-node1 ~]# cobbler sync # 修改配置都要同步
```

## ks.cfg文件简析
```
文件大部分参数含义见kickstart文章，此处只讲一些不同的地方。同时可以参考模板文件。
[root@linux-node1 kickstarts]# cat CentOS-7.1-x86_64.cfg
# Cobbler for Kickstart Configurator for CentOS 7.1 by yao zhang
install
url --url=$tree  # 这些$开头的变量都是调用配置文件里的值。
text
lang en_US.UTF-8
keyboard us
zerombr
bootloader --location=mbr --driveorder=sda --append="crashkernel=auto rhgb quiet"
# Network information
$SNIPPET('network_config')
timezone --utc Asia/Shanghai
authconfig --enableshadow --passalgo=sha512
rootpw  --iscrypted $default_password_crypted
clearpart --all --initlabel
part /boot --fstype xfs --size 1024  # CentOS7系统磁盘默认格式xfs
part swap --size 1024
part / --fstype xfs --size 1 --grow
firstboot --disable
selinux --disabled
firewall --disabled
logging --level=info
reboot
%pre
$SNIPPET('log_ks_pre')
$SNIPPET('kickstart_start')
$SNIPPET('pre_install_network_config')
# Enable installation monitoring
$SNIPPET('pre_anamon')
%end
%packages
@base
@compat-libraries
@debugging
@development
tree
nmap
sysstat
lrzsz
dos2unix
telnet
iptraf
ncurses-devel
openssl-devel
zlib-devel
OpenIPMI-tools
screen
%end
%post
systemctl disable postfix.service
%end
```

## 定制化安装
可能从学习kickstart开始就有人想怎样能够指定某台服务器使用指定ks文件，kickstart实现这功能可能比较复杂，但是Cobbler就很简单了。

区分一台服务器的最简单的方法就是物理MAC地址。
物理服务器的MAC地址在服务器上的标签上写了。

```
cobbler system add --name=oldboy --mac=00：0C：29：7F：2F：A1 --profile=CentOS-7.1-x86_64 --ip-address=10.0.0.111 --subnet=255.255.255.0 --gateway=10.0.0.2 --interface=eth0 --static=1 --hostname=oldboy.example.com --name-servers="114.114.114.114 8.8.8.8"
# --name 自定义，但不能重复
# 查看定义的列表
[root@linux-node1 ~]# cobbler system list
   oldboy
[root@linux-node1 ~]# cobbler sync
```

再次开机安装就不再询问选择了，直接安装。

## Cobbler的Web管理界面的安装与配置
已经安装cobbler-web软件。

访问网址：http://10.0.0.7/cobbler_web和https://10.0.0.7/cobbler_web

默认用户名：cobbler
默认密码 ：cobbler

```
/etc/cobbler/users.conf       # Web服务授权配置文件
/etc/cobbler/users.digest     # 用于web访问的用户名密码配置文件
[root@linux-node1 ~]# cat /etc/cobbler/users.digest
cobbler:Cobbler:a2d6bae81669d707b72c0bd9806e01f3
# 设置Cobbler web用户登陆密码
# 在Cobbler组添加cobbler用户，提示输入2遍密码确认
[root@linux-node1 ~]# htdigest /etc/cobbler/users.digest "Cobbler" cobbler
Changing password for user cobbler in realm Cobbler
New password: 123456
Re-type new password:123456
[root@linux-node1 ~]# cobbler sync
[root@linux-node1 ~]# /etc/init.d/httpd restart
停止 httpd：                                               [确定]
正在启动 httpd：                                           [确定]
[root@linux-node1 ~]# /etc/init.d/cobblerd restart
Stopping cobbler daemon:                                   [确定]
Starting cobbler daemon:                                   [确定]
```