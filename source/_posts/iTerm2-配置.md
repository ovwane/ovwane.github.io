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



配置 [itterm2-zmodem](https://github.com/aikuyun/iterm2-zmodem)

```shell
git clone https://github.com/aikuyun/iterm2-zmodem.git
cp iterm2-zmodem/{iterm2-send-zmodem.sh,iterm2-recv-zmodem.sh} /usr/local/bin/
chmod 777 /usr/local/bin/iterm2-*
rm -rf iterm2-zmodem
```



设置Iterm2的Tirgger特性，profiles->default->editProfiles->Advanced中的Tirgger

> 添加两条trigger，分别设置 Regular expression，Action，Parameters，Instant如下：

```
1.第一条
        Regular expression: rz waiting to receive.\*\*B0100
        Action: Run Silent Coprocess
        Parameters: /usr/local/bin/iterm2-send-zmodem.sh
        Instant: checked
2.第二条
        Regular expression: \*\*B00000000000000
        Action: Run Silent Coprocess
        Parameters: /usr/local/bin/iterm2-recv-zmodem.sh
        Instant: checked
```



### 设置编码

```
export LC_ALL=en_US.UTF-8  
export LANG=en_US.UTF-8
```



## 快捷键

- cmd + shift + d：一个窗口内分割为多个终端



## 参考

 [oh-my-zsh中文乱码问题](https://hearrain.com/2013/04/738) 