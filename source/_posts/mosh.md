---
title: Mosh 使用教程
date: 2017-05-14 13:54
---
[Mosh](https://mosh.org/)(mobile shell)
### 什么是Mosh

Mosh表示移动Shell(Mobile Shell)，是一个用于从客户端跨互联网连接远程服务器的命令行工具。它能用于SSH连接，但是比Secure Shell功能更多。它是一个类似于SSH而带有更多功能的应用。程序最初由Keith Winstein 编写，用于类Unix的操作系统中，发布于GNU GPL V3协议下。

Mosh最大的特点是基于UDP方式传输，支持在服务端创建一个临时的Key供客户端一次性连接，退出后失效；也支持通过SSH的配置进行认证，但数据传输本身还是自身的UDP方式。

另外，Mosh还有两个我觉得非常有用的功能

- 会话的中断不会导致当前正在前端执行的命令中断，相当于你所有的操作都是在screen命令中一样在后台执行。
- 会话在中断过后，不会立刻退出，而是启用一个计时器，当网络恢复后会自动重新连接，同时会延续之前的会话，不会重新开启一个。

#### Mosh的功能

- 它是一个支持漫游的远程终端程序
- 在所有主流的类 Unix 版本中可用，如 Linux、FreeBSD、Solaris、Mac OS X和Android
- 支持不稳定连接
- 支持智能的本地回显
- 支持用户输入的行编辑
- 响应式设计及在 wifi、3G、长距离连接下的鲁棒性
- 在IP改变后保持连接。它使用UDP代替TCP(在SSH中使用)，当连接被重置或者获得新的IP后TCP会超时，但是UDP仍然保持连接
- 在很长的时候之后恢复会话时仍然保持连接
- 没有网络延迟。立即显示用户输入和删除而没有延迟
- 像SSH那样支持一些旧的方式登录
- 包丢失处理机制

## Mosh安装配置

#### CentOS中安装Mosh

```shell
yum install -y epel-release
yum update
yum install mosh
```

检查Mosh的版本

```shell
$ mosh --version
mosh 1.3.0 [build mosh 1.3.0]
```

开启防火墙端口

```shell
firewall-cmd --permanent --zone=public --query-port=60000-60010/udp
firewall-cmd --permanent --zone=public --add-port=60000-60010/udp
firewall-cmd --permanent --zone=public --remove-port=60000-60010/udp
```

```shell
mosh-server new -i 你的IP -p 60000:60010 

mosh-server new -c 256 -s -l LANG=zh_CN.UTF-8 -l LC_CTYPE=zh_CN.UTF-8
```

#### macOS中安装

```shell
brew install mosh
```

### 总结

Mosh是一款在大多数linux发行版的仓库中可以下载的一款小工具。虽然它有一些差异尤其是安全问题和额外的需求，它的功能，比如漫游后保持连接是一个加分点。我的建议是任何一个使用SSH的Linux用户都应该试试这个程序，Mosh值得一试。

**Mosh的优缺点**

- Mosh有额外的需求，比如需要允许UDP 直接连接，这在SSH不需要。
- 动态分配的端口范围是60000-61000。第一个打开的端口是分配好的。每个连接都需要一个端口。
- 默认的端口分配是一个严重的安全问题，尤其是在生产环境中。
- 支持IPv6连接，但是不支持IPv6漫游。
- 不支持回滚。
- 不支持X11转发。
- 不支持ssh-agent转发。

```shell
mosh --ssh="ssh -i ~/keys/linode_vps_web01_ed" IP

MOSH_KEY=L08BKzyxNvJDm0P4X8nUWQ mosh-client IP 60001

mosh -ssh='ssh -vvv -i ~/keys/linode_vps_web01_ed' user@IP

ssh -i ~/keys/linode_vps_web01_ed user@IP -p 2345
```

## 参考

<https://mosh.mit.edu/>
<http://heylinux.com/archives/2955.html>
<http://blog.szrf215.com/p/4b16d7c218db>

[使用Mosh来优化SSH连接](https://www.hi-linux.com/posts/23118.html)

