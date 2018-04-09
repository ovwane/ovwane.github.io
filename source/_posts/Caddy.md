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
