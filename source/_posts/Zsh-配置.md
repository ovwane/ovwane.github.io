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

# Zsh



<!--more-->



## macOS 环境变量

系统

- `/etc/profile`

- `/etc/bashrc`

- `/etc/paths`

  

用户

- `~/.bash_profile`
- `~/.bash_login`
- `~/.profile`
- `~/.bashrc`

加载顺序：`/etc/profile`-> `/etc/paths`-> `/etc/paths.d/*`-> `~/.bash_profile`-> `~/.bash_login`-> `~/.profile`-> `~/.bashrc`

- 如果`~/.bash_profile`文件存在，则后面的几个文件就会被忽略不读了，如果`~/.bash_profile`文件不存在，才会以此类推读取后面的文件。

- `~/.bashrc`是bash shell打开的时候载入的，不受上面的规则影响。



 [MacOS设置环境变量path/paths的完全总结-foxwho(神秘狐)的领地](http://www.foxwho.com/article/184) 

 [MAC之常用操作、mac上环境变量的加载顺序、_网络_代码解释生活-CSDN博客](https://blog.csdn.net/u011146511/article/details/80860407) 



## 安装[Oh My Zsh](https://ohmyz.sh/#install) 

### macOS 

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```



### CentOS

```
yum -y install zsh && chsh -s /bin/zsh
```

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```



## 主题

**目前使用的是** ys 



## 插件

插件管理工具

安装 zinit

```shell
sh -c "$(curl -fsSL https://raw.githubusercontent.com/zdharma/zinit/master/doc/install.sh)"
```

> Ignore insecure directories and continue [y] or abort compinit [n]?: https://my.oschina.net/u/215547/blog/824106



antigen、zgen、prezto、zplug

```shell
git clone --recursive https://github.com/sorin-ionescu/prezto.git "${ZDOTDIR:-$HOME}/.zprezto"

setopt EXTENDED_GLOB
for rcfile in "${ZDOTDIR:-$HOME}"/.zprezto/runcoms/^README.md(.N); do
  ln -s "$rcfile" "${ZDOTDIR:-$HOME}/.${rcfile:t}"
done
```



### [zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md)

> 语法高亮显示

```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

### [zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

> 历史命令自动补全

```shell
git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

### [**zsh-completions**](https://github.com/zsh-users/zsh-completions)

```shell
git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-completions
```

### [zsh-history-substring-search](https://github.com/zsh-users/zsh-history-substring-search)

> 历史命令搜索

```shell
git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-search
```

### [zsh-apple-touchbar](https://github.com/zsh-users/zsh-apple-touchbar)

```shell
git clone https://github.com/zsh-users/zsh-apple-touchbar ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-apple-touchbar
```

### [autojump](https://github.com/wting/autojump)

```
brew install autojump
```

### [powerline-status](https://github.com/powerline/powerline)
```
pip install powerline-status
pip show powerline-status
```

### [powerline-fonts](https://github.com/powerline/fonts)
```
git clone https://github.com/powerline/fonts.git
./fonts/install.sh
rm -rf fonts
```



## 配置

### ~/.zshrc 文件
```shell
# 插件
plugins=(
	zsh-syntax-highlighting
	zsh-autosuggestions
	zsh-completions
	zsh-history-substring-search
	autojump
	zsh-apple-touchbar
	docker
	docker-compose
	)

# powerline
. /usr/local/lib/python3.6/site-packages/powerline/bindings/zsh/powerline.zsh
```



### 文件类型快捷方式

```
alias -s html='vim'

# #设置 md 文件都使用 Typora 打开
alias typora='open -a /Applications/Typora.app'
alias -s md=typora
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

 [在 Mac 上将 zsh 用作默认 Shell - Apple 支持](https://support.apple.com/zh-cn/HT208050) 

https://github.com/powerline/powerline
https://github.com/powerline/fonts 

[玩oh my zsh - 山夕为岁](https://zryang.github.io/2018/04/14/oh-my-zsh/) 

[Cheatsheet · ohmyzsh/ohmyzsh Wiki](https://github.com/ohmyzsh/ohmyzsh/wiki/Cheatsheet) 

[Coding style guide · ohmyzsh/ohmyzsh Wiki](https://github.com/ohmyzsh/ohmyzsh/wiki/Coding-style-guide) 

[zsh环境下如何补全docker命令 | Blog Persona](https://phaedo.github.io/blog/os/2019/07/18/zsh%E7%8E%AF%E5%A2%83%E4%B8%8B%E5%A6%82%E4%BD%95%E8%A1%A5%E5%85%A8docker%E5%91%BD%E4%BB%A4/) 

[oh-my-zsh,让你的终端从未这么爽过](https://www.jianshu.com/p/d194d29e488c)  

 [我就感觉到快 —— zsh 和 oh my zsh 冷启动速度优化 | Sukka's Blog](https://blog.skk.moe/post/make-oh-my-zsh-fly) 

 [oh-my-zsh中文乱码问题](https://hearrain.com/2013/04/738) 