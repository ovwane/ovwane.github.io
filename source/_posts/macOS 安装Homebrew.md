title: macOS 安装Homebrew
date: 2017-10-20 11:00:00
categories:
- 技术
- macOS
tags:
- macOS
- Homebrew
---
macOS 安装[Homebrew](https://brew.sh)
macOS 10.12.6
macOS 10.13

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
```
brew cask install istat-menus keepassxc google-chrome firefox firefoxdeveloperedition opera xmind mplayerx vlc macdown atom sublime-text iterm2 zoc qq charles alfred pycharm thunder wireshark youdaodict youdaonote
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

### 工作用的软件
邮箱客户端
foxmail

沟通工具
dingtalk

远程桌面
teamviewer
microsoft-remote-desktop-beta

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


### 工作用的软件
[Worktile](https://my.worktile.com/mobile)

## 软件注册码
### iStat Menus:
购买了密钥 ovwane@gmail.com

### PyCharm RubyMine WebStorm激活服务器：
http://118.89.31.228:1017

### Sublime Text:
```
—– BEGIN LICENSE —–
TwitterInc
200 User License
EA7E-890007
1D77F72E 390CDD93 4DCBA022 FAF60790
61AA12C0 A37081C5 D0316412 4584D136
94D7F7D4 95BC8C1C 527DA828 560BB037
D1EDDD8C AE7B379F 50C9D69D B35179EF
2FE898C4 8E4277A8 555CE714 E1FB0E43
D5D52613 C3D12E98 BC49967F 7652EED2
9D2D2E61 67610860 6D338B72 5CF95C69
E36B85CC 84991F19 7575D828 470A92AB
—— END LICENSE ——
```

### VMware Fusion
```
FG3TU-DDX1M-084CY-MFYQX-QC0RD
```

