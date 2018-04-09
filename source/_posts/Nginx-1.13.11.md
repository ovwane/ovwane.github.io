vim /etc/yum.repos.d/nginx.repo

```
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/mainline/centos/7/$basearch/
gpgcheck=0
enabled=1
```

`yum install nginx -y`

查看nginx版本 `nginx -v`
获取编译参数 `nginx -V`
文件位置
/usr/lib/systemd/system/nginx.service
user  nginx

```
[Unit]
Description=nginx - high performance web server
Documentation=http://nginx.org/en/docs/
After=network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStart=/usr/sbin/nginx -c /etc/nginx/nginx.conf
ExecReload=/bin/kill -s HUP $MAINPID
ExecStop=/bin/kill -s TERM $MAINPID

[Install]
WantedBy=multi-user.target
```

```bash
--prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib64/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-O2 -g -pipe -Wall -Wp,-D_FORTIFY_SOURCE=2 -fexceptions -fstack-protector-strong --param=ssp-buffer-size=4 -grecord-gcc-switches -m64 -mtune=generic -fPIC' --with-ld-opt='-Wl,-z,relro -Wl,-z,now -pie'
```

## 编译安装nginx
安装依赖环境

```bash
yum install -y make git pcre-devel zlib-devel
```

添加nginx用户和组

```
useradd -s /sbin/nologin -M nginx 
```

获取源码：

```bash
git clone -b 1.13.12 https://github.com/nginx/nginx.git
git clone https://github.com/openssl/openssl.git
git clone https://github.com/grahamedgecombe/nginx-ct.git
git clone https://github.com/google/ngx_brotli.git&&cd ngx_brotli&&git submodule update --init&&cd ..
```

编译

```bash
cd nginx

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

### 使用[acme.sh](https://github.com/Neilpang/acme.sh)工具获取ssl证书

```
curl https://get.acme.sh | sh
```

```bash
#1. DNSPod API Key and ID
export DP_Id=""
export DP_Key=""

#2.
acme.sh --issue --dns dns_dp --nginx --keylength 4096 -d quanjinlong.cn -d www.quanjinlong.cn -d blog.quanjinlong.cn

#3.
acme.sh --issue --dns dns_dp --nginx --keylength 4096 -d quanjinlong.cn -d www.quanjinlong.cn -d blog.quanjinlong.cn --keylength ec-256

#4.
acme.sh --installcert -d quanjinlong.cn -d www.quanjinlong.cn -d blog.quanjinlong.cn \
        --key-file   /data/ssl/quanjinlong.cn/quanjinlong_cn_rsa.key \
        --fullchain-file /data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_fullchain.cer \
        --reloadcmd  "systemctl restart nginx.service"
      
#4.        
acme.sh --install-cert -d quanjinlong.cn -d www.quanjinlong.cn -d blog.quanjinlong.cn \
--key-file       /data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_key.pem  \
--fullchain-file /data/ssl/quanjinlong.cn/quanjinlong_cn_rsa_fullchain_cert.pem \
--reloadcmd     "systemctl restart nginx.service"
```

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

启动

```bash
systemctl start nginx.service
systemctl restart nginx.service
systemctl stop nginx.service
systemctl status nginx.service
systemctl enable nginx.service
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