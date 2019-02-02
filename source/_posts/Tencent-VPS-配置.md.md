---
date: 2018-04-10 13:24:21
---



**配置主机名**

```shell
hostnamectl set-hostname 域名
```



**添加用户**

```shell
useradd 用户名
```



**添加 sudo 权限** [Ubuntu 设置当前用户sudo免密码](https://www.linuxidc.com/Linux/2016-12/139018.htm)

```shell
visudo
```

在`root    ALL=(ALL)       ALL` 下面一行，添加`用户名 ALL=(ALL)       NOPASSWD: ALL`。



**生成 ssh key**

```shell
ssh-keygen -o -a 100 -t ed25519 -C "邮箱" -f ~/.ssh/tencent_vps_ed
```



**添加 ssh public key**

```shell
su 用户名

mkdir ~/.ssh
vim ~/.ssh/authorized_keys

chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```



**配置 SSH**

```shell
vim /etc/ssh/sshd_config
```

```
Port 22
Protocol 2
RSAAuthentication yes
PubkeyAuthentication yes
# 禁止 root 用户通过 SSH 登录
PermitRootLogin no
# 禁用密码登录
PasswordAuthentication no
ChallengeResponseAuthentication no
UsePAM yes
UseDNS no
X11Forwarding yes
```



重启 SSH 服务

```shell
systemctl restart sshd.service
```



删除 ntp

```shell
yum list installed |grep ntp

yum -y autoremove ntp

rm -rf /etc/ntp.conf.rpmsave

yum -y autoremove ntpdate
```



更新系统 [yum upgrade](https://www.jianshu.com/p/4df7692bdc2b)

```shell
yum -y upgrade 
```



删除老版本 kernel

```shell
# 查看 kernel
rpm -qa | grep kernel  

# 重启后新 kernel 生效了，才能删除老版本的 kernel
yum -y autoremove kernel-3.10.0-862.el7
yum -y autoremove kernel-devel-3.10.0-862.el7
```



**时间同步**

[chrony](https://chrony.tuxfamily.org/documentation.html)

 [chrony软件使用说明](https://www.cnblogs.com/clsn/archive/2017/11/16/7844857.html)

```shell
# 安装
yum -y install chrony

# 启动
systemctl start chronyd.service
systemctl enable chronyd.service
systemctl status chronyd.service

# 查看同步状态
chronyc sourcestats
```



**安装常用软件**

```shell
# 工具
yum install -y lrzsz tmux wget vim git tree unzip

# 网络
yum install -y nc tcpdump mtr nmap telnet lsof net-tools sysstat 
```


## 参考

[centOS7服务管理与启动流程 - Linuxbugs - 博客园](https://www.cnblogs.com/duzhaoqi/p/7582404.html)




#优化
1. 关闭SELinux
2. 关闭firewalld
3. 最大进程数和最大文件打开数
	修改/etc/security/limits.conf文件
4. 
删除默认用户
centos

5. 更换yum源
[CentOS mirrors List](https://www.centos.org/download/mirrors/)

```
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
wget -O /etc/yum.repos.d/epel.repo http://mirrors.aliyun.com/repo/epel-7.repo

删除缓存
yum clean all
rm -rf /var/cache/yum

生成缓存
yum makecache
```


[删除软件包并删除孤立的依赖包](http://www.mamicode.com/info-detail-1553924.html)
https://segmentfault.com/q/1010000000626683

```
需要在 /etc/yum.conf 里面添加一个配置：
clean_requirements_on_remove=1

yum autoremove
```

删除postfix

```
yum -y autoremove postfix
```

```
touch /var/lock/subsys/local
/usr/local/qcloud/rps/set_rps.sh >/tmp/setRps.log 2>&1
/usr/local/qcloud/irq/net_smp_affinity.sh >/tmp/net_affinity.log 2>&1
```

删除 firewalld

删除 wpa_supplicant
```
yum -y autoremove wpa_supplicant
```

删除用户

```
/etc/passwd
/etc/shadow
/etc/group
/etc/gshadow

postfix
ntp
games
ftp
```

