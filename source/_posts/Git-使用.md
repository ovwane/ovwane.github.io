---
title: Git 使用
date: 2013-05-11 12:48:46
---

# [Git](https://git-scm.com/)



<!--more-->



### git常用命令

```shell
#克隆仓库
git clone git@github.com:ovwane/ovwane.github.io.git
#创建分支
git checkout -b blog
#切换分支
git checout blog
#查看分支
git branch
#查看所有分支
git branch -a
#删除分支
git push origin blog
git push origin -d blog
#删除子模块
git rm -r --cached themes/spfk
```



### 撤销操作

```shell
# 查看之前提交的git commit的id 
$ git log 
# 完成撤销,同时将代码恢复到前一commit_id 对应的版本 
$ git reset --hard id
# 完成Commit命令的撤销，但是不对代码修改进行撤销，可以直接通过git commit 重新提交对本地代码的修改
git reset id
```



### 更改远程分支地址

```bash
# 查看
git remote -v
```



## [Git全局递归忽略.DS_Store](http://chen-tao.github.io/2017/09/24/Git%E5%85%A8%E5%B1%80%E9%80%92%E5%BD%92%E5%BF%BD%E7%95%A5-DS-Store/)

全局配置

如果没有`~/. gitignore_global`文件，`echo`也会为你生成一个，这里的主要目的是覆盖所有可能的`OS X`版本生成的`.DS_Store`，逐一执行一次就可以了，之后`cat`一下看是否正常写入了

```shell
echo ".DS_Store" >> ~/.gitignore_global
echo "._.DS_Store" >> ~/.gitignore_global
echo "**/.DS_Store" >> ~/.gitignore_global
echo "**/._.DS_Store" >> ~/.gitignore_global
```

然后设置一下全局的配置

```shell
git config --global core.excludesfile ~/.gitignore_global
```


### git操作中文文件错误

```shell
#core.quotepath设为false的话，就不会对0x80以上的字符进行quote。中文显示正常。
git config --global core.quotepath false
```



### ~/.ssh/config 配置代理

>  [macOS 给 Git(Github) 设置代理（HTTP/SSH）](https://gist.github.com/chuyik/02d0d37a49edc162546441092efae6a1) 

```
brew install socat
```

```
Host gitlab.com
  HostName gitlab.com
  User git
  PreferredAuthentications publickey
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/keys/gitlab_rsa
  TCPKeepAlive yes
  # 走 HTTP 代理
  ProxyCommand socat - PROXY:127.0.0.1:%h:%p,proxyport=7890
  # 走 socks5 代理
  # ProxyCommand nc -v -x 127.0.0.1:1080 %h %p
```



## 参考

 [Git - Reference](https://git-scm.com/docs) 
