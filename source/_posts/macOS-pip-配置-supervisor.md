---
title: macOS pip 配置 supervisor
date: 2018-10-17 15:27:45
tags:
---



> macOS 10.13.6



etc目录在/usr/local/etc

```shell
mkdir -p /usr/local/etc/supervisor/
echo_supervisord_conf > /usr/local/etc/supervisor/supervisord.conf
```

vim /usr/local/etc/supervisor/supervisord.conf

```shell
[inet_http_server]         
port=127.0.0.1:9001

[supervisord]
logfile=/usr/local/etc/supervisor/supervisord.log
logfile_maxbytes=50MB        ; max main logfile bytes b4 rotation; default 50MB
logfile_backups=10           ; # of main logfile backups; 0 means none, default 10
loglevel=info                ; log level; default info; others: debug,warn,trace
pidfile=/usr/local/etc/supervisor/supervisord.pid

[supervisorctl]
serverurl=http://127.0.0.1:9001
serverurl=unix:///usr/local/etc/supervisor/supervisor.sock

[include]
files = /usr/local/etc/supervisor/conf.d/*.conf
```

启动supervisor

```shell
supervisord -c /usr/local/etc/supervisor/supervisord.conf
```

