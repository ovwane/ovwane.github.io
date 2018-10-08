---
title: macOS 10.12.6 Mariadb配置
date: 2017-08-08 15:59:28
---

brew tap homebrew/services

#### 安装mariadb
brew install mariadb

```
jinlong  ~  brew install mariadb
==> Downloading https://homebrew.bintray.com/bottles/mariadb-10.2.7_1.sierra.bot
######################################################################## 100.0%
==> Pouring mariadb-10.2.7_1.sierra.bottle.tar.gz
==> Caveats
A "/etc/my.cnf" from another install may interfere with a Homebrew-built
server starting up correctly.

MySQL is configured to only allow connections from localhost by default

To connect:
    mysql -uroot

To have launchd start mariadb now and restart at login:
  brew services start mariadb
Or, if you don't want/need a background service you can just run:
  mysql.server start
==> Summary
🍺  /usr/local/Cellar/mariadb/10.2.7_1: 623 files, 162.2MB
```

#### 启动mariadb
brew services start mariadb

#### 查看服务列表
brew services list

