---
date: 2018-04-14 20:35:32
---

编译安装nginx

安装依赖环境

```bash
yum install -y make gcc git pcre-devel zlib-devel
```

添加nginx用户和组

```
useradd -s /sbin/nologin -M nginx 
```

<!--more-->

获取源码：

```bash
mkdir nginx
cd nginx

git clone -b release-1.13.12 https://github.com/nginx/nginx.git
#openssl 1.1.1-pre5-dev
git clone https://github.com/openssl/openssl.git
git clone https://github.com/grahamedgecombe/nginx-ct.git
git clone https://github.com/google/ngx_brotli.git&&cd ngx_brotli&&git submodule update --init&&cd ..
```

编译

```bash
cd nginx
#开启ipv6，但编译参数没有 --with-ipv6 应该默认支持ipv6了。
auto/configure --prefix=/usr/local/nginx-1.13.11 --user=nginx --group=nginx --add-module=../ngx_brotli --add-module=../nginx-ct --with-openssl=../openssl --with-openssl-opt='enable-tls1_3 enable-weak-ssl-ciphers' --with-http_v2_module --with-http_ssl_module --with-http_gzip_static_module

make
make install
```

/usr/lib/systemd/system/nginx.service

```
cat >/usr/lib/systemd/system/nginx.service<<EOF
[Unit]
Description=nginx - high performance web server
Documentation=http://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStartPre=/usr/local/nginx-1.13.11/sbin/nginx -t -c /usr/local/nginx-1.13.11/conf/nginx.conf
ExecStart=/usr/local/nginx-1.13.11/sbin/nginx -c /usr/local/nginx-1.13.11/conf/nginx.conf
ExecReload=/usr/local/nginx-1.13.11/sbin/nginx -s reload
ExecStop=/usr/local/nginx-1.13.11/sbin/nginx -s quit
PrivateTmp=true

[Install]
WantedBy=multi-user.target
EOF
```

配置

/usr/local/nginx-1.13.11/conf/nginx.conf

```shell
user  nginx nginx;

worker_processes  2;

pid        /var/run/nginx.pid;

events {
    worker_connections  1024;
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    charset            utf-8;

    sendfile       on;

    tcp_nopush     on;
    tcp_nodelay    on;

    keepalive_timeout  60;

    gzip               on;
    gzip_vary          on;
    gzip_comp_level    6;
    gzip_buffers       16 8k;
    gzip_min_length    1000;
    gzip_proxied       any;
    gzip_disable       "msie6";
    gzip_http_version  1.0; 
    gzip_types         text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript image/svg+xml;

    brotli             on;
    brotli_comp_level  6;
    brotli_types       text/plain text/css application/json application/x-javascript text/xml application/xml application/xml+rss text/javascript application/javascript image/svg+xml;

    include            /data/nginx/conf/*.conf;
} 
```

ovwane.com.conf

mkdir -p /data/nginx/log/

mkdir -p /data/nginx/conf/

mkdir -p /data/www/ovwane.com/public



```shell
vim /data/nginx/conf/ovwane.com.conf

server {
    listen               443 ssl http2 fastopen=3 reuseport;
    listen               [::]:443 ssl http2 fastopen=3 reuseport;

    # 如果你使用了 Cloudflare 的 HTTP/2 + SPDY 补丁，记得加上 spdy
    # listen               443 ssl http2 spdy fastopen=3 reuseport;

    server_name          ovwane.com;
    server_tokens        off;

    ssl_ct               on;

    #RSA
    ssl_certificate     /data/ssl/ovwane.com/ovwane.com_rsa_fullchain.cer;
    ssl_certificate_key /data/ssl/ovwane.com/ovwane.com_rsa.key;
    ssl_ct_static_scts   /data/ssl/ovwane.com/scts;

    #ECC
    ssl_certificate     /data/ssl/ovwane.com/ovwane.com_ecc_fullchain.cer;
    ssl_certificate_key /data/ssl/ovwane.com/ovwane.com_ecc.key;
    ssl_ct_static_scts   /data/ssl/ovwane.com/scts;

    ssl_dhparam         /data/ssl/ovwane.com/dhparams.pem;

    ssl_ciphers                TLS13-AES-256-GCM-SHA384:TLS13-CHACHA20-POLY1305-SHA256:TLS13-AES-128-GCM-SHA256:TLS13-AES-128-CCM-8-SHA256:TLS13-AES-128-CCM-SHA256:EECDH+CHACHA20:EECDH+CHACHA20-draft:EECDH+ECDSA+AES128:EECDH+aRSA+AES128:RSA+AES128:EECDH+ECDSA+AES256:EECDH+aRSA+AES256:RSA+AES256:EECDH+ECDSA+3DES:EECDH+aRSA+3DES:RSA+3DES:!MD5;

    ssl_prefer_server_ciphers  on;

    ssl_protocols              TLSv1 TLSv1.1 TLSv1.2 TLSv1.3; # 增加 TLSv1.3

    ssl_session_cache          shared:SSL:50m;
    ssl_session_timeout        1d;
    ssl_session_tickets        on;

    #OCSP
    ssl_stapling               on;
    ssl_stapling_verify        on;
    #ssl_trusted_certificate

    resolver                 8.8.8.8 1.1.1.1  valid=300s;
    resolver_timeout         10s;

    access_log                 /data/nginx/log/ovwane.com.log;

    if ($request_method !~ ^(GET|HEAD|POST|OPTIONS)$ ) {
        return           444;
    }

    if ($host != 'ovwane.com' ) {
        rewrite          ^/(.*)$  https://ovwane.com/$1 permanent;
    }

    location / {
        add_header               Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";
        add_header               X-Frame-Options deny;
        add_header               X-Content-Type-Options nosniff;
        add_header               Content-Security-Policy "default-src 'none'; script-src 'unsafe-inline' 'unsafe-eval' blob:https:; img-src data: https: http://ovwane.com; style-src 'unsafe-inline' https:; child-src https:; connect-src 'self' https://translate.googleapis.com; frame-src https://disqus.com";
        add_header               Public-Key-Pins 'pin-sha256="sRHdihwgkaib1P1gxX8HFszlD+7/gTfNvuAybgLPNis="; pin-sha256="YLh1dUR9y6Kja30RrAn7JKnbQG/uEtLMkBgFF2Fuihg="; max-age=2592000; includeSubDomains';
        add_header               Cache-Control no-cache;
        #QUIC
        add_header "alt-svc" "quic=\":444\"; ma=2592000; v=\"39\"";

        root /data/www/ovwane.com/public;
        index index.html index.htm;
    }
}

server {
    listen 80;
    listen [::]:80;
    server_name ovwane.com;
    server_tokens off;

    access_log        /dev/null;

    if ($request_method !~ ^(GET|HEAD|POST)$ ) {
        return        444;
    }

    location / {
        rewrite       ^/(.*)$ https://ovwane.com/$1 permanent;
    }
}
```





### 使用[acme.sh](https://github.com/Neilpang/acme.sh)工具获取ssl证书

```
curl https://get.acme.sh | sh
```

```bash
#1. DNS he.net
export HE_Username=""
export HE_Password=""

#2.
acme.sh --issue --dns dns_he --keylength 4096 -d ovwane.com -d www.ovwane.com -d m.ovwane.com

#3.
acme.sh --issue --dns dns_he --keylength 4096 -d ovwane.com -d www.ovwane.com -d m.ovwane.com --keylength ec-256

mkdir -p /data/ssl/ovwane.com/ 

#4.
acme.sh --installcert -d ovwane.com \
        --key-file   /data/ssl/ovwane.com/ovwane.com_rsa.key \
        --fullchain-file /data/ssl/ovwane.com/ovwane.com_rsa_fullchain.cer \
        --reloadcmd  "systemctl restart nginx.service"
#5.
cp ovwane.com_ecc/fullchain.cer /data/ssl/ovwane.com/ovwane.com_ecc_fullchain.cer
cp ovwane.com_ecc/ovwane.com.key /data/ssl/ovwane.com/ovwane.com_ecc.key
```

日志自动切分

mkdir -p /data/nginx/log/

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

启动

```bash
systemctl start nginx.service
systemctl restart nginx.service
systemctl stop nginx.service
systemctl status nginx.service
systemctl enable nginx.service
```

```shell
netstat -tulpna | grep nginx
lsof -i -P|grep nginx
```



[ssl 测试工具](https://www.ssllabs.com/ssltest/)

- TLS 1.3
[本博客开始支持 TLS 1.3](https://imququ.com/post/enable-tls-1-3.html)|[CentOS 7 编译安装nginx并启用TLS1.3](https://www.coldawn.com/tag/draft-23/)

- CT
[通过 nginx-ct 启用 CT](https://imququ.com/post/certificate-transparency.html#toc-2)

[Certificate Transparency Monitor](https://ct.grahamedgecombe.com/)

```bash
yum -y install golang

wget -O ct-submit.zip -c https://github.com/grahamedgecombe/ct-submit/archive/v1.1.2.zip
unzip ct-submit.zip
cd ct-submit-1.1.2
go build
```

```
#rsa
./ct-submit-1.1.2 ct.googleapis.com/icarus </data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/icarus.sct

./ct-submit-1.1.2 ct1.digicert-ct.com/log </data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/digicert.sct

./ct-submit-1.1.2 mammoth.ct.comodo.com </data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/comodo.sct

#ecc
./ct-submit-1.1.2 ct.googleapis.com/icarus </data/ssl/quanjinlong.cn/quanjinlong_cn_ecc_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/icarus_ecc.sct

./ct-submit-1.1.2 ct1.digicert-ct.com/log </data/ssl/quanjinlong.cn/quanjinlong_cn_ecc_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/digicert_ecc.sct

./ct-submit-1.1.2 mammoth.ct.comodo.com </data/ssl/quanjinlong.cn/quanjinlong_cn_ecc_fullchain_cert.pem >/data/ssl/quanjinlong.cn/scts/comodo_ecc.sct
```

- [HSTS]()

nginx server conf

```
add_header               Strict-Transport-Security "max-age=31536000; includeSubDomains; preload";
```

- [HKPK](https://gist.github.com/esurdam/ef72f1c47be7c074499cb920683bd307)

[Chain of Trust - Let's Encrypt - Free SSL/TLS Certificates](https://letsencrypt.org/certificates/)

```
wget -O lets-encrypt-x3-cross-signed.pem https://letsencrypt.org/certs/lets-encrypt-x3-cross-signed.pem.txt

wget -O lets-encrypt-x4-cross-signed.pem https://letsencrypt.org/certs/lets-encrypt-x4-cross-signed.pem.txt
```

```
openssl x509 -noout -in lets-encrypt-x3-cross-signed.pem -pubkey | \
openssl rsa -pubin -outform der | \
openssl dgst -sha256 -binary | \
base64
```

```
openssl x509 -noout -in lets-encrypt-x4-cross-signed.pem -pubkey | \
openssl rsa -pubin -outform der | \
openssl dgst -sha256 -binary | \
base64
```

nginx server conf

```
add_header Public-Key-Pins 'pin-sha256="YLh1dUR9y6Kja30RrAn7JKnbQG/uEtLMkBgFF2Fuihg="; pin-sha256="sRHdihwgkaib1P1gxX8HFszlD+7/gTfNvuAybgLPNis=="; max-age=2592000; includeSubDomains';
```

- [ssl_dhparam](https://weakdh.org/sysadmin.html)

```
openssl dhparam -out dhparams.pem 2048
```

nginx server conf

```
ssl_dhparam dhparams.pem
```

[imquu.com-本博客 Nginx 配置之完整篇](https://imququ.com/post/my-nginx-conf.html)