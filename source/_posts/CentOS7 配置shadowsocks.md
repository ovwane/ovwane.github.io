---
date: 2018-05-08 21:11:49
---

```
pip install --upgrade pip
pip install shadowsocks

```

fast_open未启用

```shell
echo 3 > /proc/sys/net/ipv4/tcp_fastopen
```



```shell
vim /etc/shadowsocks.json

{
  "server":"",
  "server_port":,
  "local_address": "127.0.0.1",
  "local_port":1027,
  "password":"",
  "timeout":300,
  "method":"rc4-md5",
  "workers": 1
}
```

```shell
nohup sslocal -c /etc/shadowsocks.json /dev/null 2>&1 &
```

```shell
curl --socks5 127.0.0.1:1027 http://httpbin.org/ip
```

```shell
yum install -y privoxy
```

```shell
vim /etc/privoxy/config

listen-address 127.0.0.1:1087
forward-socks5t / 127.0.0.1:1027 .
```

```shell
systemctl enable privoxy.service
systemctl start privoxy.service
systemctl status privoxy.service
systemctl restart privoxy.service
```

```shell
export http_proxy=http://127.0.0.1:1087;export https_proxy=http://127.0.0.1:1087;

curl http://httpbin.org/ip
```

### 参考

[CentOS 7 安装 shadowsocks 客户端](https://brickyang.github.io/2017/01/14/CentOS-7-%E5%AE%89%E8%A3%85-Shadowsocks-%E5%AE%A2%E6%88%B7%E7%AB%AF/)

