```Shell
timedatectl set-timezone America/Los_Angeles
hostnamectl set-hostname ovwane.com
```

```shell
useradd jinlong
passwd jinlong
```

```Shell
visudo
jinlong ALL=(ALL)       ALL
```

```
ssh-keygen -t rsa -b 4096 -C "ovwane@gmail.com" -f linode_vps_rsa
```

```
mkdir ~/.ssh
vim ~/.ssh/authorized_keys

chmod 600 ~/.ssh/authorized_keys
chmod 700 ~/.ssh
```

[设置 SSH，打开密钥登录功能](https://www.cnblogs.com/handongyu/p/6386789.html)
编辑 /etc/ssh/sshd_config 文件，进行如下设置：

```
vim /etc/ssh/sshd_config

Port 22
Protocol 2
RSAAuthentication yes
PubkeyAuthentication yes
#禁止root用户通过SSH登录
PermitRootLogin no
#禁用密码登录
PasswordAuthentication no
ChallengeResponseAuthentication no
GSSAPIAuthentication no
UsePAM yes
UseDNS no
X11Forwarding no
```

最后，重启 SSH 服务：
`systemctl restart sshd.service`


#优化
1. 关闭SELinux

   ```Shell
   sed -i 's/SELINUX=enforcing/SELINUX=disabled/g' /etc/selinux/config
   ```

2. 最大进程数和最大文件打开数
  修改/etc/security/limits.conf文件

3. 
  删除默认用户
  centos

4. 更换yum源
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

[EPEL mirrors](https://admin.fedoraproject.org/mirrormanager/mirrors/EPEL/7/x86_64)

mirrors.cat.pdx.edu



yum upgrade](https://www.jianshu.com/p/4df7692bdc2b)

```
yum -y upgrade 
```
```
rpm -qa | grep kernel  

yum -y autoremove kernel-3.10.0-693.el7
yum -y autoremove kernel-3.10.0-693.2.2.el7.x86_64
```

6. 时间

  [chrony](https://chrony.tuxfamily.org/documentation.html)

[chrony软件使用说明](https://www.cnblogs.com/clsn/archive/2017/11/16/7844857.html)

```
yum -y install chrony

systemctl start chronyd.service
systemctl enable chronyd.service
systemctl status chronyd.service

查看同步状态
chronyc sourcestats
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


[centOS7服务管理与启动流程](https://www.cnblogs.com/duzhaoqi/p/7582404.html)


下载必要软件包
再安装操作系统的时候使用的最小化安装，有很多包没有安装，使用时发现好多命令没有如{vim、wget、tree...等}，下面就安装命令，可以根据需求自行调整。

```
yum -y install wget vim lrzsz screen lsof tree unzip git
# tcpdump nc mtr openssl-devel  bash-completion  nmap telnet ntpdate net-tools 
```
开启firewalld

```Shell
systemctl start firewalld.service
systemctl status firewalld.service
```

