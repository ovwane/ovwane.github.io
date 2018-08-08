---
date: 2018-08-08 20:29:08
---

[Redis](https://redis.io)

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

参考

[How to install Redis 4 on Centos 6 / 7, Ubuntu 16 and Debian 8 (Updated)](https://www.hugeserver.com/kb/install-redis-4-centos-ubuntu-debian/)

[How to Install Redis Server in CentOS and Debian Based Systems](https://www.tecmint.com/install-redis-server-in-centos-ubuntu-debian/)

[[Redis配置成系统服务（CentOS7）](http://www.cnblogs.com/sybblogs/p/5665123.html)](https://www.cnblogs.com/sybblogs/p/5665123.html)

[CentOS7源码安装Redis及配置系统服务](https://blog.csdn.net/javacspring/article/details/53618570)

[redis 配置 参数 详解](http://blog.51yip.com/nosql/1724.html)