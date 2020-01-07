---
title: macOS 安装软件
date: 2017-10-10 19:52:00
tags: macOS
---

# macOS 安装[Homebrew](https://brew.sh)

> macOS 10.12.6
>
> macOS 10.13

## 安装Homebrew
1. 安装 Command Line Tools for Xcode
```
xcode-select --install
```

2. 安装brew
```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

3. 安装[Homebrew cask](http://caskroom.github.io)
```
brew tap caskroom/cask
brew tap caskroom/versions
```

清理缓存

```shell
# brew brew cask 缓存都会清理
brew cleanup
#强制删除所有cache
rm -rf $(brew --cache)
```

## Homebrew 安装软件列表

```
brew install wget vim git tree zsh-completions nmap
```

下载工具
wget

编辑器
vim

版本控制
git

目录结构查看
tree

命令补全
[zsh-completions](http://icarus4.logdown.com/posts/177661-from-bash-to-zsh-setup-tips)
```
vim ~/.zshrc

# zsh-completions start
fpath=(/usr/local/share/zsh-completions $fpat)
# zsh-completions end

#执行命令
rm -f ~/.zcompdump; compinit
```

网络工具
nmap

## Homebrew Cask 安装软件列表
```shell
brew cask install istat-menus keepassxc google-chrome firefox firefoxdeveloperedition opera xmind mplayerx vlc macdown atom sublime-text iterm2 zoc qq charles alfred pycharm thunder wireshark youdaodict youdaonote zoomus keka archiver losslesscut
```

密码管理工具
keepassxc

网页浏览器
google-chrome
firefox
firefoxdeveloperedition
opera
vivaldi
opera
[Opera 12.16](http://get.geo.opera.com/pub/opera/mac/1216/)

思维导图
xmind

视频播放器
mplayerx
vlc

监控
istat-menus

文本编辑器
macdown
atom
sublime-text

终端远程工具
iterm2
zoc

聊天工具
qq

代理工具
charles

工作流
alfred

编程IDE
pycharm
rubymine
webstorm

下载工具
thunder

解压缩工具
the-unarchiver

百度网盘
baidunetdisk

网络工具
wireshark

翻译工具
youdaodict 

云笔记
youdaonote

音乐
neteasemusic

虚拟机
vmware-fusion
parallels

视频会议

zoom.us

视频剪切

losslesscut

### 工作用的软件
邮箱客户端
foxmail

沟通工具
dingtalk

远程桌面
teamviewer
microsoft-remote-desktop-beta

代码管理
sourcetree （git）
cornerstone (svn)

## App Store 安装软件列表
### IDE
Xcode

### 办公软件
Numbers
Keynote
Pages

### 图书软件
iBooks Author

### 聊天工具
WeChat

### 任务管理
奇妙清单

## 网站下载软件列表
### 文件备份
[Synology Cloud Station Backup](https://www.synology.cn/zh-cn/support/download/DS416play#utilities)|[坚果云](https://www.jianguoyun.com/s/downloads)

### 应用软件
[DMG Canvas](http://www.araelium.com/dmgcanvas)|[Adobe Creative Cloud](https://creative.adobe.com/products/creative-cloud)| [Logitech 游戏软件 G502](http://support.logitech.com.cn/zh_cn/product/g502-proteus-core-tunable-gaming-mouse/downloads)|[ShadowsocksX-NG](https://github.com/shadowsocks/ShadowsocksX-NG/releases)

### 视频剪辑

~~TunesKit Video Cutter~~

### 软件清理

~~App Uninstaller~~

App Cleaner & Uninstaller


### 工作用的软件
[Worktile](https://my.worktile.com/mobile)

## 软件注册码
### iStat Menus:
购买了密钥 ovwane@gmail.com

### PyCharm RubyMine WebStorm激活服务器：
http://118.89.31.228:1017
http://idea.imsxm.com

### Sublime Text:
```
----- BEGIN LICENSE -----
sgbteam
Single User License
EA7E-1153259
8891CBB9 F1513E4F 1A3405C1 A865D53F
115F202E 7B91AB2D 0D2A40ED 352B269B
76E84F0B CD69BFC7 59F2DFEF E267328F
215652A3 E88F9D8F 4C38E3BA 5B2DAAE4
969624E7 DC9CD4D5 717FB40C 1B9738CF
20B3C4F1 E917B5B3 87C38D9C ACCE7DD8
5F7EF854 86B9743C FADC04AA FB0DA5C0
F913BE58 42FEA319 F954EFDD AE881E0B
------ END LICENSE ------
```

### VMware Fusion
```
FG3TU-DDX1M-084CY-MFYQX-QC0RD
```



**查找本地缓存目录**

brew是将所有要安装的包都下载到缓存目录中，然后才进行安装，那么我们就可以通过`brew --cache`来查看是在哪个目录

```shell
brew --cache
```

**手动下载的安装包移动到缓存目录**

在执行`brew install` `brew upgrade`的时候会在控制台中打印出下载地址

```shell
#下载
wget https://services.gradle.org/distributions/gradle-4.5-all.zip

#移动到缓存目录，删除incomplete文件
mv ~/Downloads/gradle-4.5-all.zip ~/Library/Caches/Homebrew
rm -f ~/Library/Caches/Homebrew/gradle-4.5.zip.incomplete

#重命名
cd ~/Library/Caches/Homebrew
mv gradle-4.5-all.zip gradle-4.5.zip
```

**执行`brew upgrade`，安装本地的安装包。**

```shell
brew upgrade
```



## 参考

[brew使用本地安装包 | Charles's Note](https://liyang.pro/brew-install-local-package/)