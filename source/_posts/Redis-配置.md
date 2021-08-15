---
title: Redis 配置
date: 2016-12-24 16:20:31
categories:
- 技术
- Linux
tags:
- CentOS
- Redis
---

# [Redis](https://redis.io)

<!--more-->



## 安装

```shell
wget http://download.redis.io/releases/redis-4.0.9.tar.gz
tar xzf redis-4.0.9.tar.gz
cd redis-4.0.9
make
make test

cd utils
./install_server.sh
```

```shell
mkdir -p /data/redis/6379
mkdir -p /data/redis/log
mkdir /etc/redis
```

```shell
//增加redis用户组
groupadd redis
useradd -c Redis Server -s /sbin/nologin -d /var/lib/redis -g redis -G root 123
```

> reids参数解释： 
> -c 用户描述信息 
> -s 用户执行脚本，此处为安全考虑，redis用户是不允许远程登录，故使用/sbin/nologin 
> -d 用户home目录，此处无需在/home目录下创建redis子目录，故将其定位于/var/lib/redis空目录中 
> -G 扩展用户组，即表示此用户同时属于root用户组

配置

```conf
vim /etc/redis/6379.conf

bind 127.0.0.1
protected-mode yes
port 6379
tcp-backlog 511
timeout 0
tcp-keepalive 300
daemonize yes
supervised systemd
pidfile /data/redis/redis_6379.pid
loglevel notice
logfile /data/redis/log/redis_6379.log
databases 16
always-show-logo yes
save 900 1
save 300 10
save 60 10000
stop-writes-on-bgsave-error yes
rdbcompression yes
rdbchecksum yes
dbfilename dump.rdb
dir /data/redis/6379
slave-serve-stale-data yes
slave-read-only yes
repl-diskless-sync no
repl-diskless-sync-delay 5
repl-disable-tcp-nodelay no
slave-priority 100
lazyfree-lazy-eviction no
lazyfree-lazy-expire no
lazyfree-lazy-server-del no
slave-lazy-flush no
appendonly no
appendfilename "appendonly.aof"
appendfsync everysec
no-appendfsync-on-rewrite no
auto-aof-rewrite-percentage 100
auto-aof-rewrite-min-size 64mb
aof-load-truncated yes
aof-use-rdb-preamble no
lua-time-limit 5000
slowlog-log-slower-than 10000
slowlog-max-len 128
latency-monitor-threshold 0
notify-keyspace-events ""
hash-max-ziplist-entries 512
hash-max-ziplist-value 64
list-max-ziplist-size -2
list-compress-depth 0
set-max-intset-entries 512
zset-max-ziplist-entries 128
zset-max-ziplist-value 64
hll-sparse-max-bytes 3000
activerehashing yes
client-output-buffer-limit normal 0 0 0
client-output-buffer-limit slave 256mb 64mb 60
client-output-buffer-limit pubsub 32mb 8mb 60
hz 10
aof-rewrite-incremental-fsync yes
```



Systemd

```
vim /usr/lib/systemd/system/redis.service

[Unit]
Description=Redis In-Memory Data Store
After=network.target
[Service]
User=root
Group=root
PIDFile=/data/redis/redis_6379.pid
ExecStart=/usr/local/bin/redis-server /etc/redis/6379.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/usr/local/bin/redis-cli -h 127.0.0.1 -p 6379 -a 密码 shutdown
#ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true
Restart=always
Type=forking
[Install]
WantedBy=multi-user.target
```

启动

```shell

systemctl enable redis.service
systemctl start redis.service
systemctl restart redis.service
systemctl stop redis.service
systemctl status redis.service
```

#### 设置redis密码

```
vim /etc/redis.conf

requirepass 密码
```

登陆有密码的Redis：

在登录的时候的时候输入密码：

   ```shell
redis-cli -p 6379 -a test123
   ```

   先登陆后验证：

 ```shell
redis-cli -p 6379

redis 127.0.0.1:6379> auth test123
 ```

查看密码

```redis
config get requirepass
```

## Docker 运行 Redis

```bash
docker run -d -p 6379:6379 -v ~/docker/redis/:/data  --name redis redis:6.2.5-alpine3.14 redis-server --appendonly yes
```



## Redis的多种启动方式

[Redis的多种启动方式比较！](http://renzhiyuan.blog.51cto.com/10433137/1881202)

>有感:Redis玩了许久时间，真心感觉启动方式还是自己定义的方便！

1）直接启动和关闭：（配置文件默认）

```
开启：redis-server &（&后台运行）
#daemonize yes（也可配置文件修改此参数）
关闭：redis-cli shutdown or killall -9 redis-server
```

2）指定配置文件启动：

```
redis-server /etc/redis.conf(配置文件可自己定义)
如果更改了redis默认端口：
redis-cli shutdown (-p 端口)
redis-cli shutdown (-p 端口) （-a 认证密码）
```

3）自己定义启动文件并配置(推荐）

```
[root@redis1 ~]# cpredis-2.8.24/utils/redis_init_script /etc/init.d/redis
注册为系统服务：
[root@redis1 ~]# sed -i '2i #chkconfig:2345 80 90' /etc/init.d/redis
[root@redis1 ~]# chkconfig --add redis
```
修改配置文件（因为路径自己定义,sed也可以）

```
REDISPORT=7000  #注意slave端口自己定义即可
EXEC=/usr/local/redis/bin/redis-server
CLIEXEC=/usr/local/redis/bin/redis-cli
 
PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/etc/redis/${REDISPORT}.conf"
```

脚本如下（自己配置的，大家也可在此基础上进行修改）

```
#!/bin/sh
#chkconfig: 2345 80 90
# Simple Redis init.d script conceivedto work on Linux systems
# as it does use of the /procfilesystem.
  
REDISPORT=7000
EXEC=/usr/local/redis/bin/redis-server
CLIEXEC=/usr/local/redis/bin/redis-cli
  
PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/etc/redis/${REDISPORT}.conf"
  
case "$1" in
   start)
       if [ -f $PIDFILE ]
       then
                echo -e "\033[31m $PIDFILEexists, process is already running or crashed \033[0m"
       else
                echo -e "\033[32m Waitingfor Redis to start...\033[0m"
                $EXEC $CONF
                if [ $? -eq 0 ]
                then
                echo -e "\033[32m Redis isalready started successfully! \033[0m"
                else
                echo -e "\033[31m Redisstart fail \033[0m"
                fi
       fi
       ;;
   stop)
       if [ ! -f $PIDFILE ]
       then
                echo -e "\033[31m $PIDFILEdoes not exist, process is not running \033[0m"
       else
                PID=$(cat $PIDFILE)
                echo -e "\033[32m Waitingfor Redis to stop ... \033[0m"
                $CLIEXEC -p $REDISPORT  shutdown
                while [ -x /proc/${PID} ] 
                do
                    echo -e "\033[32mWaiting for Redis to shutdown ... \033[0m"
                   sleep 1
                done
                echo -e "\033[32m Redis isalready stopped successfully! \033[0m"
       fi
       ;;
   status)
                 ps aux|grep redis-server|grep-v grep &>/dev/null
                 if [ $? -eq 0 ]
                 then
                 echo -e "\033[32m Redisis running... \033[0m"
                 else
                 echo -e "\033[31m Redisis already stopped \033[0m"
                 fi
                 ;;
   restart)
               $CLIEXEC -p $REDISPORT  shutdown
               if [ $? -eq 0 ]
               then
               echo -e "\033[32m Redis isalready stopped successfully! \033[0m"
               else 
               echo -e "\033[31m Redisstop fail \033[0m"
               fi
               $EXEC $CONF
               if [ $? -eq 0 ]
               then
               echo -e "\033[32m Redis isalready started successfully! \033[0m"
               else
               echo -e "\033[31m Redisstart fail \033[0m"
               fi
               ;;
    *)
     echo "the usage is service redis start|stop|status|restart"
     esac
```

查看redis状态，启动，关闭，重启

```
[root@redis1 ~]# /etc/init.d/redis status
 Redis is running... 
[root@redis1 ~]# /etc/init.d/redis stop
 Waiting for Redis to stop ... 
 Waiting for Redis to shutdown ... 
 Redis is already stopped successfully! 
[root@redis1 ~]# /etc/init.d/redis start
 Waiting for Redis to start...
 Redis is already started successfully! 
[root@redis1 ~]# /etc/init.d/redis restart
 Redis is already stopped successfully! 
 Redis is already started successfully! 
[root@redis1 ~]#
[root@redis1 ~]# ps aux|grep redis-server|grep -v grep
root       2881  0.1  0.1 128296  1692 ?        Ssl  12:45   0:01 /usr/local/redis/bin/redis-server *:7000              
[root@redis1 ~]#
```



## Redis数据持久化

[Redis数据持久化](http://renzhiyuan.blog.51cto.com/10433137/1881895)

Redis是个支持持久化的内存数据库，redis需要经常将内存中的数据同步到磁盘来保证持久化。

1、RDB方式（Snapshotting默认快照方式）：
1.1）配置：

```
save 900 1       #在900秒(15分钟)之后，如果至少有1个key发生变化，则dump内存快照。
save 300 10      #在300秒(5分钟)之后，如果至少有10个key发生变化，则dump内存快照。
save 60 10000    #在60秒(1分钟)之后，如果至少有10000个key发生变化，则dump内存快照。
```

1.2）工作原理：
1.2.1）Redis使用fork函数复制一份当前进程（父进程）的副本（子进程）；
1.2.2）父进程继续接收并处理客户端发来的命令，而子进程开始将内存中的数据写入硬盘中的临时文件；
1.2.3）当子进程写入完所有数据后会用该临时文件替换旧的RDB文件，至此一次快照操作完成。

1.3）查看dump.rdb:

```
127.0.0.1:7000> config get dir
1) "dir"
2) "/usr/local/redis/db"
127.0.0.1:7000>
```

1.4）优点：
1.4.1）RDB是个非常紧凑的文件，保存了redis在某个时间点上的数据集，使得我们可以通过定时备份RDB文件来实现Redis数据库备份和灾难恢复，也可以将其传送到其他的数据中心用于保存。
1.4.2）RDB可以最大化redis的性能，执行RDB持久化时只需要fork一个子进程，并由子进程进行持久化工作，父进程不需要处理任何磁盘I/O操作。
1.4.3）RDB在恢复大数据集时比AOF要快，启动效率要高许多。
1.4.4）RDB文件是经过压缩（可以配置rdbcompression参数以禁用压缩节省CPU占用）的二进制格式，所以占用的空间会小于内存中的数据大小，更加利于传输。

1.5）缺点：
1.5.1）每次快照持久化都是将内存数据完整写入到磁盘一次，并不是增量的只同步增数据。如果数据量大的话，而且写操作比较多，必然会引起大量的磁盘io操作，可能会严重影响性能。
1.5.2）快照方式是在一定间隔时间做一次的，所以如果redis意外down掉的话，就会丢失最后一次快照后的所有修改，有一些数据丢失的风险。
1.5.2）client的save或者bgsave命令通知redis做一次快照持久化不推荐。

```
127.0.0.1:7000> save
OK
```
原因：save操作是在主线程中保存快照的，由于redis是用一个主线程来处理所有 client的请求，这种方式会阻塞所有client请求。所以不推荐使用。

2、Append-only file（缩写aof）的方式：
2.1）配置

```
appendonly yes  
#开启aof
appendfilename "appendonly.aof"  
#名字
# appendfsync always   
# 每次执行写入都会执行同步，最安全也最慢
appendfsync everysec   
# 每秒执行一次同步操作
# appendfsync no 
#不主动进行同步操作，而是完全交由操作系统来做（即每30秒一次),最快也最不安全。
#配置写入AOF文件后，要求系统刷新硬盘缓存的机制
auto-aof-rewrite-percentage 100  
# 当目前的AOF文件大小超过上一次重写时的AOF文件大小的百分之多少时会再次进行重写，如果之前没有重写过，则以启动时的AOF文件大小为依据
auto-aof-rewrite-min-size 64mb   
# 允许重写的最小AOF文件大小
```

2.2）工作原理：
2.2.1）redis调用fork ，现在有父子两个进程
2.2.2）子进程根据内存中的数据库快照，往临时文件中写入重建数据库状态的命令
2.2.3）父进程继续处理client请求，除了把写命令写入到原来的aof文件中。同时把收到的写命令缓存起来。这样就能保证如果子进程重写失败的话并不会出问题。
2.2.4.）当子进程把快照内容写入已命令方式写到临时文件中后，子进程发信号通知父进程。然后父进程把缓存的写命令也写入到临时文件。
2.2.5）现在父进程可以使用临时文件替换老的aof文件，并重命名，后面收到的写命令也开始往新的aof文件中追加。

2.3）查看aof
```
127.0.0.1:7000> config get dir
1) "dir"
2) "/usr/local/redis/db"
127.0.0.1:7000>
[root@redis1 db]# ll
总用量 8
-rw-r--r-- 1 root root 146 2月   1 08:36 appendonly.aof
-rw-r--r-- 1 root root  74 2月   1 09:20 dump.rdb
[root@redis1 db]#
```

2.4）优点：
2.4.1）该机制可以带来更高的数据安全性，即数据持久性。
2.4.2）由于该机制对日志文件的写入操作采用的是append模式，因此在写入过程中即使出现宕机现象，也不会破坏日志文件中已经存在的内容。然而如果我们本次操作只是写入了一半数据就出现了系统崩溃问题，不用担心，在Redis下一次启动之前，我们可以通过redis-check-aof工具来帮助我们解决数据一致性的问题。
2.4.3）如果日志过大，Redis可以自动启用rewrite机制。即Redis以append模式不断的将修改数据写入到老的磁盘文件中，同时Redis还会创建一个新的文件用于记录此期间有哪些修改命令被执行。因此在进行rewrite切换时可以更好的保证数据安全性。
2.4.4） AOF包含一个格式清晰、易于理解的日志文件用于记录所有的修改操作。事实上，也可以通过该文件完成数据的重建。

2.5：缺点：
2.5.1）对于相同数量的数据集而言，AOF文件通常要大于RDB文件，持久化文件会变的越来越大。
2.5.2） 根据同步策略的不同，AOF在运行效率上往往会慢于RDB。

3、其它
虚拟内存方式和diskstore方式。：（不建议，而且虚拟内存据说2.4版本后弃用，diskstore也不常用）
相关配置

```
持久化文件会变的越来越大。
vm-enabled yes          
#开启vm功能
vm-swap-file /tmp/redis.swap   
#交换出来的value保存的文件路径/tmp/redis.swap
vm-max-memory 1000000  
#redis使用的最大内存上限，超过上限后redis开始交换value到磁盘文件中
vm-page-size 32        
#每个页面的大小32个字节
vm-pages 134217728     
#最多使用在文件中使用多少页面,交换文件的大小 = vm-page-size * vm-pages
vm-max-threads 4       
#用于执行value对象换入换出的工作线程数量，0表示不使用工作线程
```

总结：
1、Redis允许同时开启AOF和RDB，既保证了数据安全又使得进行备份等操作十分容易。重新启动Redis后Redis会使用AOF文件来恢复数据，因为AOF方式的持久化可能丢失的数据更少。

2、读写分离 通过复制可以实现读写分离以提高服务器的负载能力。在常见的场景中，读的频率大于写，当单机的Redis无法应付大量的读请求时（尤其是较耗资源的请求，比如SORT命令等）可以通过复制功能建立多个从数据库，主数据库只进行写操作，而从数据库负责读操作。

3、从数据库持久化 持久化通常相对比较耗时，为了提高性能，可以通过复制功能建立一个（或若干个）从数据库，并在从数据库中启用持久化，同时在主数据库禁用持久化。当从数据库崩溃时重启后主数据库会自动将数据同步过来，所以无需担心数据丢失。而当主数据库崩溃时，需要在从数据库中使用SLAVEOF NO ONE命令将从数据库提升成主数据库继续服务，并在原来的主数据库启动后使用SLAVEOF命令将其设置成新的主数据库的从数据库，即可将数据同步回来。



## Redis 常用命令

[Redis 常用命令](http://zengestudy.blog.51cto.com/1702365/1854097)

Redis命令有两种类型：
1）键值相关命令
2）服务相关命令



### 键值相关命令

keys：返回满足给定pattern的所有key

```
127.0.0.1:6379> keys *
127.0.0.1:6379> keys my*
```

2、exists：确认一个key是否存在，存在返回1，否则返回0

```
127.0.0.1:6379> exists mylist
127.0.0.1:6379> exists my
```

3、del：删除一个key，删除成功返回1

```
127.0.0.1:6379> del name
(integer) 1'
127.0.0.1:6379> exists name
(integer) 0
```

4、expire：设置一个key的过期时间

```
127.0.0.1:6379> expire age 10
(integer) 1
127.0.0.1:6379> ttl age
(integer) 7
127.0.0.1:6379> ttl age
(integer) 5
127.0.0.1:6379> ttl age
(integer) 4
127.0.0.1:6379> ttl age
(integer) 3
127.0.0.1:6379> ttl age
(integer) 2
127.0.0.1:6379> ttl age
(integer) 1
127.0.0.1:6379> ttl age
(integer) 1
127.0.0.1:6379> ttl age
(integer) -2
127.0.0.1:6379> ttl age
(nil)
```

5、move：将当前数据库中的key转移到其他数据库中

```
127.0.0.1:6379> select 0  //select 选择数据库
OK
127.0.0.1:6379> set age 10
OK
127.0.0.1:6379> get age
"10"
127.0.0.1:6379> move age 1
(integer) 1
127.0.0.1:6379> get age
(nil)
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> get age
"10"
```

6、persist：移除给定key的过期时间，取消成功返回1

```
127.0.0.1:6379[1]> expire age 200
(integer) 1
127.0.0.1:6379[1]> ttl age
(integer) 196
127.0.0.1:6379[1]> persist age
(integer) 1
127.0.0.1:6379[1]> ttl age
(integer) -1
```

7、rename：重命名key
```
127.0.0.1:6379> keys *
127.0.0.1:6379> rename mail email
OK
127.0.0.1:6379> keys *
```

8、type：返回值的类型,如果key不存在则返回none

```
127.0.0.1:6379> type mylist
list
127.0.0.1:6379> type name  
none
127.0.0.1:6379> type zeng
string
127.0.0.1:6379> type use
hash
```



### 服务器相关命令

1、ping：检测连接是否存活

```
127.0.0.1:6379> ping 
PONG
```

2、echo：在命令行输出指定信息

```
127.0.0.1:6379> echo "hello,world"
"hello,world"
```

3、select：选择数据库

4、quit、exit：退出命令行

5、dbsize：返回当前数据库中key的数目

```
127.0.0.1:6379> keys *
1) "name"
2) "age"
127.0.0.1:6379> dbsize
(integer) 2
```

6、info：获取服务器的信息和统计

```
127.0.0.1:6379> info
# Server
redis_version:3.2.1
```

7、config get：实时转储收到的请求

8、flushdb：删除当前选择数据库中的所有key

```
127.0.0.1:6379> dbsize 
(integer) 2
127.0.0.1:6379> flushdb 
OK
127.0.0.1:6379> dbsize
(integer) 0
127.0.0.1:6379> keys *
(empty list or set)
```

9、flushall：删除所有数据库中的所有key

```
127.0.0.1:6379> select 0
OK
127.0.0.1:6379> keys *
(empty list or set)
127.0.0.1:6379> set name zeng
OK
127.0.0.1:6379> dbsize
(integer) 1
127.0.0.1:6379> select 1
OK
127.0.0.1:6379[1]> flushall
OK
127.0.0.1:6379[1]> select 0
OK
127.0.0.1:6379> keys *
(empty list or set)
```



## Redis 图形客户端

[Redis Desktop Manager](http://www.redisdesktop.com/)

### 编译 rdm

1.  安装 Xcode 和 Xcode build tools
2.  安装 Homebrew
3.  安装依赖 `brew install openssl cmake python3`
4.  安装 git
5.  下载源码 `git clone --recursive https://github.com/uglide/RedisDesktopManager.git -b 2019.5 rdm && cd ./rdm`
6. `cd ./src && cp ./resources/Info.plist.sample ./resources/Info.plist`
7.  安装 Qt 和 Qt Creator http://download.qt.io/official_releases/qt/5.9/5.9.6/

#### ![qt](/img/redis/qt.png)

#### Qt Creator 内操作

打开 ./src/rdm.pro

![](/img/redis/1.png)

管理包

![](/img/redis/2.png) 

查看设置

![](/img/redis/3.png)

选择类型

![](/img/redis/4.png)

修改 rdm.pro 内容，生成 .app 文件。
```
# 注释 debug: CONFIG-=app_bundle
#debug: CONFIG-=app_bundle
```
![](/img/redis/5.png)

选择 release 
![](/img/redis/6.png)

build 项目
![](/img/redis/7.png)

查看输出，没有报错
![](/img/redis/8.png)

生成的 app 文件
![](/img/redis/9.png)


在别的电脑上使用rdm，使用 macdeployqt 移动依赖文件 。

`~/Qt5.9.6/5.9.6/clang_64/bin/macdeployqt`

```
cd rdm/bin/osx/
macdeployqt Redis\ Desktop\ Manager.app -qmldir=../../../src/qml
```



## 参考

 [Install - Redis Desktop Manager](http://docs.redisdesktop.com/en/latest/install/#build-from-source) 

 [Mac OS X下编译Redis Desktop Manager(RDM) | 一水的博客 | Onew Blog](https://onew.me/2018/03/29/mac-compile-RDM/) 

 [mac源码编译RedisDesktopManager 2019.3_redis,rdm_xie408979832的专栏-CSDN博客](https://blog.csdn.net/xie408979832/article/details/100566984) 

 [源码编译Redis Desktop Manager - 看我](https://kany.me/2019/10/10/compile-redis-desktop-manager/)

  [源码打包RedisDesktop MacOS版 - 简书](https://www.jianshu.com/p/f4a392b4fd75)  

 [MAC 下编译 RedisDesktopManager 最新版_zhangatle的博客-CSDN博客](https://blog.csdn.net/zhangatle/article/details/101671697) 

[How to install Redis 4 on Centos 6 / 7, Ubuntu 16 and Debian 8 (Updated)](https://www.hugeserver.com/kb/install-redis-4-centos-ubuntu-debian/)

[How to Install Redis Server in CentOS and Debian Based Systems](https://www.tecmint.com/install-redis-server-in-centos-ubuntu-debian/)

[[Redis配置成系统服务（CentOS7）](http://www.cnblogs.com/sybblogs/p/5665123.html)](https://www.cnblogs.com/sybblogs/p/5665123.html)

[CentOS7源码安装Redis及配置系统服务](https://blog.csdn.net/javacspring/article/details/53618570)

[redis 配置 参数 详解](http://blog.51yip.com/nosql/1724.html)