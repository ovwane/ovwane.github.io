日志自动切分

vim /etc/logrotate.d/nginx

```
/data/nginx/log/*.log {
    su root root
    daily
    rotate 5
    missingok
    notifempty
    sharedscripts
    dateext
    postrotate
        if [ -f /var/run/nginx.pid ]; then
            kill -USR1 `cat /var/run/nginx.pid`
        fi
    endscript
}
```

手动执行

```
/usr/sbin/logrotate -f /etc/logrotate.d/nginx
```
