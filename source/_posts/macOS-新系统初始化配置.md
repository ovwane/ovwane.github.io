---
title: macOS 新系统初始化配置
date: 2018-05-05 06:51:28
tags: macOS
---

## 环境

1. 安装[Homebrew](https://brew.sh)

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. 安装[Homebrew Cask](https://formulae.brew.sh/cask/)

```shell
$ brew tap caskroom/cask
$ brew tap caskroom/versions
```

3. 安装[mas](https://github.com/mas-cli/mas)

```shell
$ brew install mas
```



## 安装软件

### Homebrew

```shell
$ brew install vim wget git
```

### Homebrew Cask

```shell
$ brew cask install typora google-chrome firefox sublime-text keepassxc qq youdaodict youdaonote iina istat-menus baidunetdisk
```

```shell
$ brew cask install firefox-developer-edition iterm2 zoc wireshark charles pycharm goland
```

### mas

```shell
$ mas install 409201541 409203825 409183694 410628904 836500024
```

409201541 Pages

409203825 Numbers

409183694 Keynote

410628904 Wunderlist 

836500024 WeChat 



## 手动安装软件

[坚果云](https://www.jianguoyun.com/s/downloads)|[Adobe Creative Cloud](https://creative.adobe.com/products/download/creative-cloud)



#### 激活软件

[Charles 4.2.5 Mac and Win通用破解文件](https://www.52pojie.cn/thread-725112-1-1.html)

/Applications/Charles.app/Contents/Java/charles.jar



### 安装rm删除保护软件

>https://github.com/HuangXiZhou/blog/issues/39

```
brew install trash
```

替换别名

```
alias rm=trash
```

