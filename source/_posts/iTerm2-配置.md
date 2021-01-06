---
title: iTerm2 配置
date: 2018-05-05 12:15:39
---

# [iTerm2](https://iterm2.com/)



<!--more-->



## [iTerm2主题](http://iterm2colorschemes.com/)

**Solarized Dark Higher Contrast**

```shell
wget https://raw.githubusercontent.com/mbadolato/iTerm2-Color-Schemes/master/schemes/Solarized%20Dark%20Higher%20Contrast.itermcolors
```

导入主题

iTerm2->Preferences->Profiles->Color

Color Presets->import

目前没有使用iTerm2主题



## 配置

### 保存日志

1.Preferences->Profiles->Terminal->Scrollback lines 

勾选 Unlimited scrollback

2.Preferences->Profiles->Session->Miscellaneous

日志存放路径：`~/logs/iTerm2`



### 传输文件

安装 lrzsz

```shell
brew install lrzsz
```



配置 [itterm2-zmodem](https://github.com/aikuyun/iterm2-zmodem)

```shell
git clone https://github.com/aikuyun/iterm2-zmodem.git && \
cp iterm2-zmodem/{iterm2-send-zmodem.sh,iterm2-recv-zmodem.sh} /usr/local/bin/ && \
chmod 777 /usr/local/bin/iterm2-* && \
rm -rf iterm2-zmodem
```



设置 iTerm2 的 Tirgger 特性，profiles->default->editProfiles->Advanced中的Tirgger

> 添加两条trigger，分别设置 Regular expression，Action，Parameters，Instant如下

1.第一条

```
Regular expression: rz waiting to receive.\*\*B0100
Action: Run Silent Coprocess
Parameters: /usr/local/bin/iterm2-send-zmodem.sh
Instant: checked
```

2.第二条

```
Regular expression: \*\*B00000000000000
Action: Run Silent Coprocess
Parameters: /usr/local/bin/iterm2-recv-zmodem.sh
Instant: checked
```



## 快捷键

分割視窗功能：

- `command` + `d` 垂直分割視窗
- `command` + `shift` + `d` 水平分割視窗
- `command` + 方向鍵 切換分割視窗
- `command` + `t` 開新的 tab
- `command` + 方向鍵 切換tab



## 参考

 [oh-my-zsh中文乱码问题](https://hearrain.com/2013/04/738) 

 [客製我的 CLI — 終於稍微搞懂 iTerm + ZSH. 剛開始寫程式的時候跟 terminal 很不熟，隨便在網路上找了一些… | by Fred Hung | Medium](https://spreered.medium.com/%E5%AE%A2%E8%A3%BD%E6%88%91%E7%9A%84-cli-%E7%B5%82%E6%96%BC%E7%A8%8D%E5%BE%AE%E6%90%9E%E6%87%82-iterm-zsh-d3feed27f664) 

