---
date: 2018-08-08 16:50:50
---

[Caddy 部署实践](https://github.com/Unknwon/wuwen.org/issues/11)

[Caddy](https://github.com/mholt/caddy/releases/)

[Caddy Web服务器QUIC部署](https://www.wolfcstech.com/2017/01/09/Caddy%20Web%E6%9C%8D%E5%8A%A1%E5%99%A8QUIC%E9%83%A8%E7%BD%B2/)

[QUIC在微博中的落地思考](http://www.infoq.com/cn/news/2018/03/weibo-quic)

[Examples for using Caddy Web Server](https://github.com/caddyserver/examples)

[本站开启支持 QUIC 的方法与配置](https://liudanking.com/beautiful-life/%E6%9C%AC%E7%AB%99%E5%BC%80%E5%90%AF%E6%94%AF%E6%8C%81-quic-%E7%9A%84%E6%96%B9%E6%B3%95%E4%B8%8E%E9%85%8D%E7%BD%AE/)

[使用caddy配合nginx支持QUIC
](https://blog.sharpbai.com/2018/02/%E4%BD%BF%E7%94%A8caddy%E9%85%8D%E5%90%88nginx%E6%94%AF%E6%8C%81quic/)

```
wget https://github.com/mholt/caddy/releases/download/v0.10.12/caddy_v0.10.12_linux_amd64.tar.gz
```
```
tar xf caddy_v0.10.12_linux_amd64.tar.gz

mv caddy /usr/local/bin/
```

/usr/local/bin/caddy -quic -log stdout -agree=true -conf=/data/caddy/Caddyfile -root=/var/tmp --port=32457



mkdir -p /data/caddy/log/

```shell
vim /data/caddy/Caddyfile

ovwane.com:444 {
        tls /data/ssl/ovwane.com/ovwane.com_rsa_fullchain.cer /data/ssl/ovwane.com/ovwane.com_rsa.key
        root /data/www/ovwane.com/public
        gzip
        log /data/caddy/log/ovwane.com.log
}

```



```shell
vim /usr/lib/systemd/system/caddy.service

[Unit]
Description=Caddy HTTP/2 web server
Documentation=https://caddyserver.com/docs
After=network-online.target
Wants=network-online.target systemd-networkd-wait-online.service

[Service]
Restart=on-abnormal

; User and group the process will run as.
;User=caddy
;Group=caddy

; Letsencrypt-issued certificates will be written to this directory.
Environment=CADDYPATH=/data/caddy

; Always set "-root" to something safe in case it gets forgotten in the Caddyfile.
ExecStart=/usr/local/bin/caddy -quic -log stdout -agree=true -conf=/data/caddy/Caddyfile -root=/var/tmp
ExecReload=/bin/kill -USR1 $MAINPID

; Use graceful shutdown with a reasonable timeout
KillMode=mixed
KillSignal=SIGQUIT
TimeoutStopSec=5s

; Limit the number of file descriptors; see `man systemd.exec` for more limit settings.
LimitNOFILE=1048576
; Unmodified caddy is not expected to use more than that.
LimitNPROC=512

; Use private /tmp and /var/tmp, which are discarded after caddy stops.
PrivateTmp=true
; Use a minimal /dev (May bring additional security if switched to 'true', but it may not work on Raspberry Pi's or other devices, so it has been disabled in this dist.)
PrivateDevices=false
; Hide /home, /root, and /run/user. Nobody will steal your SSH-keys.
ProtectHome=true
; Make /usr, /boot, /etc and possibly some more folders read-only.
ProtectSystem=full
; … except /etc/ssl/caddy, because we want Letsencrypt-certificates there.
;   This merely retains r/w access rights, it does not add any new. Must still be writable on the host!
ReadWriteDirectories=/data/caddy

; The following additional security directives only work with systemd v229 or later.
; They further restrict privileges that can be gained by caddy. Uncomment if you like.
; Note that you may have to add capabilities required by any plugins in use.
;CapabilityBoundingSet=CAP_NET_BIND_SERVICE
;AmbientCapabilities=CAP_NET_BIND_SERVICE
;NoNewPrivileges=true

[Install]
WantedBy=multi-user.target
```

```shell
systemctl enable caddy.service
systemctl start caddy.service
systemctl stop caddy.service
systemctl status caddy.service
```

```shell
netstat -lnp --inet6 | grep -F caddy
```




日志自动切分

vim /etc/logrotate.d/caddy

```
/data/caddy/log/*.log {
    su root root
    daily
    rotate 5
    missingok
    notifempty
    sharedscripts
    dateext
    postrotate
        if [ -f /var/run/caddy.pid ]; then
            kill -USR1 `cat /var/run/caddy.pid`
        fi
    endscript
}
```

手动执行

```
/usr/sbin/logrotate -f /etc/logrotate.d/caddy
```
