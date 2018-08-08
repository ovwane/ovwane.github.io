---
date:2017-10-16 08:25:23
---

# macOS 安装数据库服务

>macOS 10.12.6
>macOS 10.13

#### 位置
```
# 二进制文件
/usr/local/opt/
usr/local/Cellar

#  临时文件
/usr/local/var/
```

#### 安装 MariaDB
***

Error: The `brew link` step did not complete successfully
The formula built, but is not symlinked into /usr/local
Could not symlink share/man/man8/mysqld.8
/usr/local/share/man/man8 is not writable.

You can try again using:
  brew link mariadb

# 解决方法
$ brew link mariadb
Linking /usr/local/Cellar/mariadb/10.2.9...
Error: Could not symlink share/man/man8/mysqld.8
/usr/local/share/man/man8 is not writable.

# 提示‘/usr/local/share/man/man8’ 不能写入
```
sudo chown -R $(whoami) /usr/local/share/man/man8
```

# 再次执行 
```
brew link mariadb
​````
MAC下 brew link qt 出现的一些问题](http://www.cnblogs.com/o-din/p/5929037.html)
***
```
brew install mariadb
brew services start mariadb

# iTerm2 输入 mysql 找不到这个命令
/usr/local/Cellar/mariadb/10.2.9/bin
/usr/local/opt/mariadb/bin

# 有时候没有自己建立sock文件，需要手动建立。
touch /tmp/mysql.sock

#### 安装 Redis

```shell
brew install redis

brew services start redis

redis-server /usr/local/etc/redis.conf
```

#### 安装 MongoDB

```shell
brew install mongodb

brew services start mongodb

mongod --config /usr/local/etc/mongod.conf
```

