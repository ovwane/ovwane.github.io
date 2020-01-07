---
title: iTerm2 配置
date: 2018-05-05 12:15:39
---

## [iTerm2主题](http://iterm2colorschemes.com/)

**Solarized Dark Higher Contrast**

```shell
wget https://raw.githubusercontent.com/mbadolato/iTerm2-Color-Schemes/master/schemes/Solarized%20Dark%20Higher%20Contrast.itermcolors
```

导入主题

iTerm2->Preferences->Profiles->Color

Color Presets->import

目前没有使用iTerm2主题



### 传输文件

安装 lrzsz

```shell
brew install lrzsz
```



配置 itterm2-zmodem

```shell
git clone https://github.com/aikuyun/iterm2-zmodem.git
cp iterm2-zmodem/{iterm2-send-zmodem.sh,iterm2-recv-zmodem.sh} /usr/local/bin/
chmod 777 /usr/local/bin/iterm2-*
rm -r iterm2-zmodem
```



## 快捷键

- cmd + shift + d：一个窗口内分割为多个终端