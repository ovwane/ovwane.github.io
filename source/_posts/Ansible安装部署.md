title: Ansible安装部署
date: 2016-03-15 08:24:13
categories:
- 技术
- Linux
tags:
- CentOS
- Ansible
---
[Ansible安装部署](http://www.showerlee.com/archives/1649)

# 简介
Ansible是一种集成IT系统的配置管理, 应用部署, 执行特定任务的开源平台. 它基于Python语言实现, 部署只需在主控端部署Ansible环境, 被控端无需安装代理工具, 只需打开SSH, 让主控端通过SSH秘钥认证对其进行所有的管理监控操作. 相对于SaltStack, 它除了利用SSH安全传输, 无需在客户端进行任何配置, 而且它有一个很庞大的用户群体以及丰富的API, 相对适合部署到数量比较大且对系统软件安装要求比较严格的集群中.
更多配置参考: https://github.com/ansible 
官方文档: http://docs.ansible.com/ansible

# 环境
安装环境:
System: Centos 6.7 x64
Master: master.example.com 
Minion: client01.example.com
Minion: client02.example.com

# 配置
## 环境部署
1. 关闭iptables和SELINUX

```
# service iptables stop
# setenforce 0
# vi /etc/sysconfig/selinux
SELINUX=disabled
```

2. Master端安装EPEL第三方yum源

```
# rpm -Uvh http://ftp.linux.ncsu.edu/pub/epel/6/i386/epel-release-6-8.noarch.rpm
```

3. 安装Ansible
a. yum

```
# yum install ansible -y
```

b.pip
```
pip install ansible
```

4. 添加环境变量以便vi能正常显示中文注释.

```
# vi /etc/profile
添加:
export LC_ALL=en_US.UTF-8
export LANG=en_US.UTF-8
export LANGUAGE=en_US.UTF-8
# source /etc/profile
```

## 初始配置
1. 修改主机及组配置

```
# cd /etc/ansible
# cp hosts hosts.bak
# cat /dev/null > hosts
# vi /etc/ansible/hosts
[webservers]
client01.example.com
client02.example.com
[nginx01]
client01.example.com
[nginx02]
client02.example.com
```

2. 配置SSH秘钥认证

```
# yum install ssh* -y
# ssh-keygen -t rsa
```

同步公钥文件id_rsa.pub到目标主机

```
# ssh-copy-id -i /root/.ssh/id_rsa.pub root@client01.example.com
# ssh-copy-id -i /root/.ssh/id_rsa.pub root@client02.example.com
```

校验SSH免密码配置是否成功.

```
# ssh root@client02.example.com
```
如直接进入则配置完成.

3. 定义主机与组
所有定义的主机与组规则都在/etc/Ansible/hosts下.
常见的写法:

```
192.168.1.21:2135 定义一个IP为192.168.1.21, SSH端口为2135的主机.
```
```
jumper ansible_ssh_port=22 ansible_ssh_host=192.168.1.50 定义一个别名为jumper, SSH端口为22, IP为192.168.1.50的主机. 
组成员主机名称范例:
[webservers]
www[001:006].example.com
[dbservers]
db-[a:f].example.com
```

4. 定义主机变量
主机可以指定变量, 后面可以供Playbooks调用

```
[atlanta]
host1 http_port=80 maxRequestsPerChild=808
host2 http_port=8080 maxRequestsPerChild=909
5.定义组变量
[atlanta]
host1
host2

[atlanta:vars]
ntp_server=ntp.atlanta.example.com
proxy=proxy.atlanta.example.com
```

6. 匹配目标
重启webservers组所有SSH服务.

```
# ansible webservers -m service -a "name=sshd state=restarted"
```


## Ansible常用模块及API
1. 远程命令模块
command: 执行远程主机SHELL命令:

```
# ansible webservers -m command -a "free -m"
```

script: 远程执行MASTER本地SHELL脚本.(类似scp+shell)

````
# echo "df -h" > ~/test.sh
# ansible webservers -m script -a "~/test.sh"
```

2. copy模块
实现主控端向目标主机拷贝文件, 类似scp功能.
该实例实现~/test.sh文件至webservers组目标主机/tmp下, 并更新文件owner和group

```
# ansible webservers -m copy -a "src=~/test.sh dest=/tmp/ owner=root group=root mode=0755"
# ansible webservers -m copy -a "src=~/test.sh dest=/tmp/ owner=root group=root mode=0755"
```

3. stat模块
获取远程文件状态信息, 包括atime, ctime, mtime, md5, uid, gid等信息.

```
# ansible webservers -m stat -a "path=/etc/sysctl.conf"
```

4. get_url模块
实现在远程主机下载指定URL到本地.

```
# ansible webservers -m get_url -a "url=http://www.showerlee.com dest=/tmp/index.html mode=0400 force=yes"
```

5. yum模块
Linux包管理平台操作,  常见都会有yum和apt, 此处会调用yum管理模式

```
# ansible servers -m yum -a "name=curl state=latest"
```

6. cron模块
远程主机crontab配置

```
# ansible webservers -m cron -a "name='check dir' hour='5,2' job='ls -alh > /dev/null'"
```

7. service模块
远程主机系统服务管理

```
# ansible webservers -m service -a "name=crond state=stopped"
# ansible webservers -m service -a "name=crond state=restarted"
# ansible webservers -m service -a "name=crond state=reloaded"
```

8. user服务模块
远程主机系统用户管理

```
添加用户:
# ansible webservers -m user -a "name=johnd comment='John Doe'"
删除用户:
# ansible webservers -m user -a "name=johnd state=absent remove=yes"
```

## playbook介绍
playbook是一个不同于使用Ansible命令行执行方式的模式, 其功能是将大量命令行配置集成到一起形成一个可定制的多主机配置管理部署工具.
它通过YAML格式定义, 可以实现向多台主机的分发应用部署.
以下给大家详细介绍一个针对nginx嵌套复用结构的
playbook部署实例:

1. 构建目录结构

```
# cd /etc/ansible/
# mkdir group_vars
# mkdir roles
```

2. 定义host

```
# vi /etc/ansible/hosts
[webservers]
client01.example.com
client02.example.com
[nginx01]
client01.example.com
[nginx02]
client02.example.com
```

3. 定义变量

```
# vi /etc/ansible/group_vars/nginx01
worker_processes: 4
num_cpus: 4
max_open_file: 65506
root: /data
remote_user: root
# vi /etc/ansible/group_vars/nginx02
worker_processes: 2
num_cpus: 2
max_open_file: 35506
root: /www
remote_user: root
```
Tips:这里在group_vars下定义的文件名必须对应hosts文件下的group标签, 通过这里定义的不同参数从而部署不同类型的主机配置.

4. 创建roles入口文件

```
# vi /etc/ansible/site.yml
- hosts: webservers
  roles:
  - base_env
- hosts: nginx01
  roles:
  - nginx01
- hosts: nginx02
  roles:
  - nginx02
```
Tips: 这里的roles:下的字符串需对应roles目录下的目录名.

5. 定义全局role base_env
创建目录结构

```
# mkdir -p /etc/ansible/roles/base_env/tasks 
# vi /etc/ansible/roles/base_env/tasks/main.yml
# 将EPEL的yum源配置文件传送到客户端
- name: Create the contains common plays that will run on all nodes 
  copy: src=epel.repo dest=/etc/yum.repos.d/epel.repo
- name: Create the GPG key for EPEL
  copy: src=RPM-GPG-KEY-EPEL-6 dest=/etc/pki/rpm-gpg
  
# 关闭SELINUX
- name: test to see if selling is running
  command: getenforce
  register: sestatus
  changed_when: false

# 删除iptables默认规则并保存
- name: remove the default iptables rules
  command: iptables -F
- name: save iptables rules
  command: service iptables save
将对应需要拷贝到远程的文件复制到base_env/files目录下
# mkdir -p  /etc/ansible/roles/base_env/files
# cp /etc/yum.repos.d/epel.repo /etc/ansible/roles/base_env/files
# cp /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-6 /etc/ansible/roles/base_env/files
```

6. 定义nginx01和ngnix02 role
创建目录结构

```
# mkdir -p /etc/ansible/roles/nginx{01,02}
# mkdir -p /etc/ansible/roles/nginx01/tasks
# mkdir -p /etc/ansible/roles/nginx02/tasks
# vi /etc/ansible/roles/nginx01/tasks/main.yml
 # 安装nginx最新版本
- name: ensure nginx is at the latest version
  yum: pkg=nginx state=latest

# 将nginx配置文件传送到远程目录
- name: write the nginx config file
  template: src=nginx.conf dest=/etc/nginx/nginx.conf
  notify: restart nginx # 重启nginx

# 创建nginx根目录
- name: Create Web Root
  file: dest={{ root }} mode=775 state=directory owner=nginx group=nginx
  notify: reload nginx
- name: ensure nginx is running
  service: name=nginx state=restarted
# cp /home/ansible/roles/nginx01/tasks/main.yml /home/ansible/roles/nginx02/tasks/main.yml
```

7. 定义files

```
# mkdir -p /etc/ansible/roles/nginx01/templates
# mkdir -p /etc/ansible/roles/nginx02/templates
# vi /etc/ansible/roles/nginx01/templates/nginx.conf
# For more information on configuration, see: 

user              nginx;  
worker_processes  {{ worker_processes }};  
{% if num_cpus == 2 %}  
worker_cpu_affinity 01 10;  
{% elif num_cpus == 4 %}  
worker_cpu_affinity 1000 0100 0010 0001;  
{% elif num_cpus >= 8 %}  
worker_cpu_affinity 00000001 00000010 00000100 00001000 00010000 00100000 01000000 10000000;  
{% else %}  
worker_cpu_affinity 1000 0100 0010 0001;  
{% endif %}  
worker_rlimit_nofile {{ max_open_file }};  
  
error_log  /var/log/nginx/error.log;  
#error_log  /var/log/nginx/error.log  notice;  
#error_log  /var/log/nginx/error.log  info;  
  
pid        /var/run/nginx.pid;  
  
events {  
    worker_connections  {{ max_open_file }};  
}  
  
  
http {  
    include       /etc/nginx/mime.types;  
    default_type  application/octet-stream;  
  
    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '  
                      '$status $body_bytes_sent "$http_referer" '  
                      '"$http_user_agent" "$http_x_forwarded_for"';  
  
    access_log  /var/log/nginx/access.log  main;  
  
    sendfile        on;  
    #tcp_nopush     on;  
  
    #keepalive_timeout  0;  
    keepalive_timeout  65;  
  
    #gzip  on;  
      
    # Load config files from the /etc/nginx/conf.d directory  
    # The default server is in conf.d/default.conf  
    #include /etc/nginx/conf.d/*.conf;  
    server {  
        listen       80 default_server;  
        server_name  _;  
  
        #charset koi8-r;  
  
        #access_log  logs/host.access.log  main;  
  
        location / {  
            root   {{ root }};  
            index  index.html index.htm;  
        }  
  
        error_page  404              /404.html;  
        location = /404.html {  
            root   /usr/share/nginx/html;  
        }  
  
        # redirect server error pages to the static page /50x.html  
        #  
        error_page   500 502 503 504  /50x.html;  
        location = /50x.html {  
            root   /usr/share/nginx/html;  
        }  
  
    }  
  
} 
```
Tip: worker_processes, num_cpus, max_open_file, root等参数会调用group_vars目录下配置文件中相应的变量值

```
# cp /etc/ansible/roles/nginx01/templates/nginx.conf  /etc/ansible/roles/nginx02/templates/nginx.conf
```

8. 执行playbook

```
# ansible-playbook -i /etc/ansible/hosts /etc/ansible/site.yml -f 10
```
Tips: -f 为启动10个并行进程执行playbook, -i 定义inventory host文件, site.yml 为入口文件 

最终部署目录结构如下
```
# tree /etc/ansible/
/etc/ansible/
├── ansible.cfg
├── group_vars
│   ├── nginx01
│   └── nginx02
├── hosts
├── hosts.bak
├── roles
│   ├── base_env
│   │   ├── files
│   │   │   ├── epel.repo
│   │   │   └── RPM-GPG-KEY-EPEL-6
│   │   └── tasks
│   │       └── main.yml
│   ├── nginx01
│   │   ├── tasks
│   │   │   └── main.yml
│   │   └── templates
│   │       └── nginx.conf
│   └── nginx02
│       ├── tasks
│       │   └── main.yml
│       └── templates
│           └── nginx.conf
└── site.yml

11 directories, 13 files
```