---
title: ssh 使用
date: 2020-04-07 15:57:31
tags:
---

# SSH

Secure Shell（SSH）

[OpenSSH](https://www.openssh.com/)



<!--more-->

生成密钥

```
ssh-keygen -t rsa -C 邮箱  -f 密钥存放路径
```



## 客户端配置

~/.ssh/config

```shell
Host test
  # 主机名
	HostName shell.test.com
	# 用户名
	User root
	# 端口
	Port 22
	# 添加到 ssh-agent
	AddKeysToAgent yes
	# 密钥文件
	IdentityFile
```



登录服务器

```
ssh test
```



ssh-agent

```
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```



## 参考

 [SSH Config 那些你所知道和不知道的事 | Deepzz's Blog](https://deepzz.com/post/how-to-setup-ssh-config.html) 

 [ssh_config](https://www.freebsd.org/cgi/man.cgi?query=ssh_config) 