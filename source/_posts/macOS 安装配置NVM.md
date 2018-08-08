---
title: macOS 安装配置NVM
date: 2016-10-03 10:06:00
categories:
- 技术
- macOS
tags:
- macOS
- Hexo
---

macOS 安装配置NVM
macOS 10.12.6
macOS 10.13

### 安装[NVM(Node Version Manager)](https://github.com/creationix/nvm)
***
需要访问[NVM](https://github.com/creationix/nvm#install-script)站点查看最新版的安装NVM shell脚本。
***
```
#安装 NVM  # 此脚本不一定是最新的。请访问NVM站点查看最新版本的NVM。
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.5/install.sh | bash

#检查 ~/.zshrc 文件内是否有下面的内容，没有的话添加。

#NVM start
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
#NVM end
```

### 安装Node.js 
```
#查看可以安装的node版本
nvm ls-remote

#安装 node stable
nvm install stable

#安装 node v8.2.1
nvm install 8.2.1 # 安装的npm版本是5.3.0
#设置默认 node
nvm alias default 8.2.1
```

## 参考

1.http://www.cnblogs.com/Don/p/4672287.html
2.http://www.kancloud.cn/summer/nodejs-install/71975
3.https://npm.taobao.org/
4.http://cnodejs.org/topic/5338c5db7cbade005b023c98