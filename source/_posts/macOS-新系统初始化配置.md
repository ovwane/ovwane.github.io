---
title: macOS 新系统初始化配置
date: 2018-05-05 06:51:28
tags: macOS
---

# macOS 新系统初始化配置

制作U盘安装盘



<!--more-->



## 配置

安装 [Homebrew](https://brew.sh)

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```



安装[Homebrew Cask](https://formulae.brew.sh/cask/)

```shell
brew tap homebrew/cask && brew tap homebrew/cask-versions
```



安装[mas](https://github.com/mas-cli/mas)

```shell
brew install mas
```



## 安装软件

### Homebrew

```shell
brew install vim wget git tmux ffmpeg
```

>文件编辑器 vim
>
>版本控制 git
>
>Tmux 
>
>视频编辑 ffmpeg



### Homebrew Cask

```shell
brew install --cask keepassxc iina google-chrome chromium firefox firefox-developer-edition squirrel keka keka istat-menus typora marginnote baidunetdisk visual-studio-code pycharm-ce sublime-text android-file-transfer iterm2 sourcetree
```

>密码管理 keepassxc
>
>视频播放器 iina
>
>浏览器 google-chrome chromium firefox firefox-developer-edition
>
>輸入法 squirrel
>
>思维导图 Internally, Xmind Zen is now Xmind 2020.
>
>解压缩 keka
>
>硬件资源监控 istat-menus
>
>思维导图 xmind-zen
>
>Markdown typora
>
>阅读 marginnote
>
>百度云盘 baidunetdisk
>
>IDE visual-studio-code pycharm-ce sublime-text
>
>Android 文件傳輸助手 android-file-transfer
>
>终端 iterm2
>
>Tmux 
>
>版本控制 Git 图像工具 sourcetree



### MAS

```shell
mas install 497799835 409201541 409203825 409183694 836500024 664513913
```

> 497799835 Xcode
>
> 409201541 Pages
>
> 409203825 Numbers
>
> 409183694 Keynote
>
> 836500024 WeChat 
>
> 664513913 Futubull- Trade Stock & Option



## 手动安装软件

- 防火墙：[Little Snitch](https://www.obdev.at/products/littlesnitch/download.html)
- [clashX](https://github.com/yichengchen/clashX/releases)

- [坚果云](https://www.jianguoyun.com/s/downloads)

- [Adobe Creative Cloud](https://creative.adobe.com/products/download/creative-cloud)



### 激活软件

[Charles 4.2.5 Mac and Win通用破解文件](https://www.52pojie.cn/thread-725112-1-1.html)

/Applications/Charles.app/Contents/Java/charles.jar



## 其他

### 安装rm删除保护软件

>https://github.com/HuangXiZhou/blog/issues/39

```
brew install trash
```

替换别名

```
alias rm=trash
```



## 系統偏好設置

- 拖动：輔助功能-›指针控制-›触控板选项-›启用拖移，三指拖移