---
title: Hexo 配置
date: 2017-10-08 13:27:00
categories:
- 技术
- macOS
tags:
- macOS
- Hexo
---

Hexo](https://hexo.io)
macOS 10.12.6
macOS 10.13

## 安装依赖环境
### 安装git
```
brew install git
```

#### 配置git
```shell
#添加git用户姓名和邮箱地址
git config --global user.name "Jinlong Quan"
git config --global user.email "ovwane@gmail.com"

#查看配置信息
git config --global --list
```

### 配置SSH
[参考](http://blog.csdn.net/system1024/article/details/52044900)
#### 生成SSH key
```shell
ssh-keygen -t rsa -b 4096 -C "ovwane@gmail.com" -f ~/.ssh/hexo_github_rsa
ssh-keygen -t rsa -b 4096 -C "ovwane@gmail.com" -f ~/.ssh/hexo_coding_net_rsa
```

#### 配置访问权限
```shell
vim ~/.ssh/config
# github.com
Host github.com
HostName github.com
PreferredAuthentications publickey
IdentityFile ~/.ssh/hexo_github_rsa

# git.coding.net
Host git.coding.net
HostName git.coding.net
PreferredAuthentications publickey
IdentityFile ~/.ssh/hexo_coding_net_rsa
```

#### 测试SSH
```shell
ssh -T git@github.com
ssh -T git@git.coding.net
```

### 安装[NVM(Node Version Manager)](https://github.com/creationix/nvm)

### 安装Node.js 

### 安装Hexo
```
#安装Hexo
npm install hexo-cli -g

#卸载Hexo
npm uninstall hexo-cli -g

#查看Hexo版本
hexo -version
```

## 使用Hexo
### 部署Hexo
```
cd ~/projects
hexo init blog.ovwane.me
cd blog.ovwane.me
npm install
安装Hexo git插件
npm install hexo-deployer-git --save
```

### 添加Hexo主题[spfk](https://github.com/luuman/hexo-theme-spfk)
```
cd blog.ovwane.me

#下载spfk主题
git clone https://github.com/luuman/hexo-theme-spfk.git themes/spfk

#设置主题为spfk
vim _config.yml
theme: spfk
```
[参考](https://luuman.github.io/2015/12/27/Hexo/HexoTheme/)

### 使用Hexo
```
#进入博客目录下
cd blog

#新建一篇文章
hexo new

#清除缓存文件
hexo clean

#生成静态文件
hexo generate

#本地测试
hexo server

#部署到网站
hexo deploy
```

## Hexo备份
### Hexo备份初始化
***此步骤只有新建hexo项目的时候需要执行。***

```
初始化git
git init
git checkout -b hexo
git add .
git commit -m “…”

git remote add origin git@github.com:ovwane/ovwane.github.io.git

git remote add origin git@git.coding.net:ovwane/ovwane.coding.me.git

git push origin hexo
```

### 安装[Hexo备份插件](https://github.com/coneycode/hexo-git-backup)
#### 安装Hexo备份插件
```
npm install hexo-git-backup --save
```

#### 配置Hexo备份插件
```
vim _config.yml
backup:
    type: git
    theme: sfpk
    message: update xxx
    repository:
       git@github.com:ovwane/ovwane.github.io.git,hexo
       git@git.coding.net:ovwane/ovwane.coding.me.git,hexo
```

#### 使用Hexo备份插件
```
hexo backup

hexo b -m "添加注释"
```

## Hexo插件
### [hexo-filter-flowchart](https://github.com/bubkoo/hexo-filter-flowchart)
>Generate flowchart diagrams for Hexo.

```
npm install --save hexo-filter-flowchart

# vim _config.yml
# Extensions
## flowchart diagrams
flowchart:
  # raphael:   # optional, the source url of raphael.js
  # flowchart: # optional, the source url of flowchart.js
  options: # options used for `drawSVG`
```

### [hexo-filter-sequence](https://github.com/bubkoo/hexo-filter-sequence)
>Generate UML sequence diagrams for Hexo.

```
npm install --save hexo-filter-sequence

# vim _config.yml
# Extensions
## UML sequence
sequence:
  # webfont:     # optional, the source url of webfontloader.js
  # snap:        # optional, the source url of snap.svg.js
  # underscore:  # optional, the source url of underscore.js
  # sequence:    # optional, the source url of sequence-diagram.js
  # css: # optional, the url for css, such as hand drawn theme 
  options: 
    theme: 
    css_class:
```

### [hexo-tag-mermaid](https://github.com/threeq/hexo-tag-mermaid)
```
# 这个命令无法使用
npm install hexo-tag-mermaid --save

npm install https://github.com/threeq/hexo-tag-mermaid.git --save
```
### [hexo-tag-plantuml](https://github.com/threeq/hexo-tag-plantuml)

## Hexo部署文章步骤
```
hexo clean

hexo generate

hexo deploy

hexo backup -m "添加注释"
```

## Hexo 
### hexo不解析README.md文件
```
_config.yml
skip_render:
  - README.md

source目录新建README文件
```
