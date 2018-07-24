---
title: CentOS7 Shadowsocks配置文档
date: 2016-12-29 16:02:46
tags: CentOS
---

# Shadowsocks服务器配置文档

为Debian / Ubuntu 安装shadowsocks服务端：
apt-get install python-pip
pip install shadowsocks
为CentOs 安装shadowsocks服务端;
yum install python-setuptools
easy_install pip
pip install shadowsocks


在 /etc目录下创建  shadowsocks.json 文件，将下面的内容放进去：
{
"server":"my_server_ip",
"server_port":8388,
"local_address": "127.0.0.1",
"local_port":1080,
"password":"mypassword",
"timeout":300,
"method":"aes-256-cfb",
"fast_open": false,
"workers": 1
}


每个字段的的解释：
server   服务端监听的地址，服务端可填写 0.0.0.0
server_port     服务器的端口(只要不与现有的端口冲突，随你填写了，我填8137)
local_address     本地监听的地址，直接写127.0.0.1
local_port     本地的端口，随便写，只要不冲突，我填的是1345
password     你的shadowsocks连接密码
timeout     超时时间，单位秒
method     加密方式。默认是: "aes-256-cfb", 详见：see https://github.com/clowwindy/shadowsocks/wiki/Encryption
workers    应该是进程数，这个我没该，大家可以改后看看进程是否增多。不理解的化，就别改了，这个参数只有unix/linux下可用。
然后启动运行 shadowsocks服务器端：
ssserver -c /etc/shadowsocks.json
此时不要关闭终端命令操作窗口，

后台运行
CentOS：
运行命令：
sudo yum install python-pip supervisor
sudo pip install shadowsocks
编辑 /etc/supervisord.conf 在末尾添加:
[program:shadowsocks]
command=ssserver -c /etc/shadowsocks.json
autorestart=true
user=nobody
运行：
sudo chkconfig --add supervisord  //添加开机启动supervisor服务守护进程   systemctl enable supervisord.service
sudo chkconfig supervisord on
service supervisord start   //官方Git上的写错了，将“supervisord”少了个d，否则提示supervisor: unrecognized  service，意思是不能识别该服务
supervisorctl reload  //可以通过该命令重启shadowsocks。
Supervisord 是后台管理服务器, 用来依据配置文件的策略管理后台守护进程, 它会随系统自动启动
Supervisorctl 用于管理员向后台管理程序发送 启动/重启/停止 等指令;
下面一步貌似可有可无，我没在iptables上加这条规则，没遇到问题：
-A INPUT -m state --state NEW -m tcp -p tcp --dport your_server_port -j ACCEPT
//其中的your_server_port表示你刚才的shadowsocks的服务器的端口。表示允许连接vps的这个端口。