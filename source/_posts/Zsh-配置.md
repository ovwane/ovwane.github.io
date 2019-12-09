---
title: Zsh 配置
date: 2017-10-08 10:36:00
categories:
- 技术
- macOS
tags:
- macOS
- Oh My Zsh
- Zsh
---

## 安装[Oh My Zsh](http://ohmyz.sh/)

> macOS 10.12.6
> macOS 10.13

## macOS 安装 [Oh My Zsh](https://github.com/robbyrussell/oh-my-zsh)
```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

网站：http://ohmyz.sh/ | https://github.com/robbyrussell/oh-my-zsh

## **主题**

**目前使用的是** ys 



## 插件

### 安装[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md)
```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

### 安装[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

```shell
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

### 安装[**zsh-completions**](https://github.com/zsh-users/zsh-completions)

```shell
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-completions
```

### [zsh-history-substring-search](https://github.com/zsh-users/zsh-history-substring-search)

```shell
git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-search
```

### [zsh-apple-touchbar](https://github.com/zsh-users/zsh-apple-touchbar)

```shell
git clone https://github.com/zsh-users/zsh-apple-touchbar ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-apple-touchbar
```

### 安装[autojump](https://github.com/wting/autojump)

```
brew install autojump
```

### 安装[powerline-status](https://github.com/powerline/powerline)
```
pip install powerline-status
pip show powerline-status
```

### 安装[powerline-fonts](https://github.com/powerline/fonts)
```
git clone https://github.com/powerline/fonts.git
./fonts/install.sh
rm -rf fonts
```



## 参数配置

### ~/.zshrc 文件
```shell
# 插件
plugins=(
	zsh-syntax-highlighting
	zsh-autosuggestions
	zsh-completions
	zsh-history-substring-search
	
	zsh-apple-touchbar
	)

# powerline
. /usr/local/lib/python3.6/site-packages/powerline/bindings/zsh/powerline.zsh
```

### powerline vim配置 .vimrc
```
set rtp+=/usr/local/lib/python3.6/dist-packages/powerline/bindings/vim
set laststatus=2
set t_Co=256
```
[powerline安装参考](http://blog.topspeedsnail.com/archives/2652)

网站
https://github.com/wting/autojump

#### powerline-status

pip3 install powerline-status

pip3 show powerline-status

安装字体
git clone https://github.com/powerline/fonts.git
./fonts/install.sh
rm -rf fonts

应用配置

.zshrc

. /usr/local/lib/python3.6/site-packages/powerline/bindings/zsh/powerline.zsh

.vimrc

set rtp+=/usr/local/lib/python3.6/dist-packages/powerline/bindings/vim
set laststatus=2
set t_Co=256

Bash配置

```
vim /.bash_profile
. /usr/local/lib/python3.6/dist-packages/site-packages/powerline/bindings/bash/powerline.sh

立即生效
source ~/.bash_profile
```



## 参考

[ITerm2配色方案](https://www.jianshu.com/p/33deff6b8a63)

https://github.com/powerline/powerline
https://github.com/powerline/fonts