---
title: macOS 使用
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



**http://dhq.me/mac-apt-get-homebrew**



显示帮助信息

brew -h



Homebrew的版本

brew -v



列出Homebrew的建议或警告信息

brew doctor



列出已安装的软件包

brew list



更新Homebrew软件包

brew update



用浏览器打开package主页（package 为空则打开 Homebrew 主页）

1	brew home package



显示软件包内容信息

brew info package



显示包依赖

brew deps package



查找有没有想要安装的软件包（支持模糊查找）

brew search package



软件包的安装选项

brew options package



安装软件包

brew install package



如果想查看安装过程中执行的命令或者是编译信息，可以在 "install" 后面加一个 "-v" 参数：

brew install -v package



卸载软件包

brew uninstall package



用 Homebrew 安装第三方工具软件包，例如用 homebrew 安装官方缺省的php

brew tap josegonzalez/php



如果软件包出了新版本，可以用 upgrade 更新过时的软件包（缺省 package 参数，则为全部更新）：

brew upgrade package



清理之前安装的旧版本数据：

brew cleanup --force -s

rm -rf $(brew --cache)



更多详细的用法说明可以在终端输入"man brew"查看。



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



## 快捷键

文件替身：cmd+option+拖动文件

1、分窗口操作：shift+command+d（横向）command+d（竖向）

2、查找和粘贴：command+f，呼出查找功能，tab 键选中找到的文本，option+enter 粘贴

3、自动完成：command+; 根据上下文呼出自动完成窗口，上下键选择

4、粘贴历史：shift+command+h5、回放功能：option+command+b

6、全屏：command+enter

7、光标去哪了？command+/

8、Expose Tabs：Option+Command+E



## **Mac** 开发者常用的工具

**http://www.oschina.net/news/53946/mac-dev-tools**

在写 Mac 程序员的十个武器之前，我决定先讲一个故事，关于 Mac 和爱情的。（你们不是问 Mac 和爱情有个鸟关系吗？）

从前有一个孩子叫做小明，他不是高帅富，与高大上也毫无瓜葛，只有低调、无聊和内涵。他住在全国房价最贵的城市，租着最贵的单间，写着各种垃圾或垃圾回收的代码，干着程序员这份前途若有若无的职业，一切都朝着注定孤独一生的方向发展着，如果没有变数的话。

终于有一天他的朋友小强为他介绍了另一位朋友，这个朋友不是女朋友， 而是一款笔记本，笔记本的名字叫做Macbook Pro。见到 Mac 小明似乎遇到了久违的情人，呆滞的双眼放出绿油油的光芒，他花掉了所有的积蓄购买了这款笔记本，开始没日没夜的学习 iOS 和 OS X 开发的相关知识。

他在写 Java 代码的间隙写 Objective-C，在编译 Java 的同时构建 IPA，在运行完 Web Server 之后运行 iOS 虚拟机。每个清晨和夜晚他都在编程……他与 Mac 相依相偎，他们是最好的朋友。

终于有一天，他掌握了 Mac 的一部分奥秘，他编写出了自己的第一个 iOS App，花了99美元申请了开发者账户，传到了 App Store 上。又过了一段时间，他告诉他的技术主管：我要去远行。于是他去了另一个房价很贵的城市，带着增长了75%的薪资，从此杳无音讯。

两年后，小强去那个城市看望小明，发现小明身边除了升级的视网膜屏 Macbook 之外，还多了一个水灵灵的女朋友，小强和他的女朋友握了握手，发现是真人，小强觉得很欣慰。小明告诉小强，他现在是公司 iOS 开发组的 Team leader，还和女朋友一起买了套小房子，他们准备，从此幸福的生活在一起……

这就是 Mac 和爱情的故事，这是一个真实的故事，故事的主角不是我。我用 Mac 的时候孩子已经两岁了，没有机会去完成这样一个美丽的爱情故事，是我毕生的遗憾。

今天的文章到此结束。

喂喂，说好的十个 Mac 工具呢？好吧，没看到这只是上吗？

再回答一个问题：问：是不是买了 Mac 就会变得很有钱？

答：错，这当然是个伪命题，真实的情况是：

1、Mac 本来就比其他品牌的笔记本贵不少，一般情况下有钱人才会买。

2、不是有钱人的，买了 Mac 天天抱着看各种动作片和爱情片，一样无法改变注孤生的命运。

**下**

以前在 Mac 指引系列里写过一个工具列表，主要是面向普通 Mac 用户的，完整文章已经收录到纸版《MacTalk·人生元编程》中。今天的文章主要是面向程序员的，有重合，但侧重点不同。

大部分用户第一次使用 Mac 都会有个短暂的情绪反转。打开包装后马上为 Mac 精美的硬件工艺击节赞叹，进入OS X 之后随即陷入一种蛋蛋的忧伤，因为，用了十几年的开始菜单不见袅！妈妈开始菜单不见袅肿么办？这时候需要的是：淡定和冷静！

要清楚的认识到，我们寻找的不是开始按钮，而是程序入口，任何一个操 作系统，用户要做的事情并不是找到开始菜单，而是找到程序，然后打开它们完成自己的工作。在 Mac 里，完成这个职责的最佳角色不是 Dock，而是 Alfred。所以我的建议是，任何用户进入 OS X 之后，第一步就是去 App Store 下载 Alfred。普通用户使用免费版就够了，开发人员可以购买 Powerpack，物超所值。

（一）Alfred 是 Mac 平台上最为传奇的效率工具，用一篇长文来介绍都不为过，幸好 Mac 君在之前已经写过了，回复「alfred」阅读。

Mac 对原生 Shell 的支持是无数程序员喜爱 Mac 的理由之一，程序员用 Mac 而不用 Shell，基本等于自断一臂，威力将大打折扣。Shell 并非凭空而来，它的入口是终端工具。OS X自带的终端工具虽然不错，但是和 iTerm 2一比，就逊色很多了。

（二）iTerm2 是 OS X 下一款开源免费的的终端工具，我基本用它替代了原生的 Terminal。网址：[http://www.iterm2.com](http://www.iterm2.com/)

功能还有很多，多用多体会。

另外，很多朋友说自己的终端一直是黑白的，如何换成彩电？在用户目录的.profile里加上这两行即可：export CLICOLOR=1export LSCOLORS=gxfxcxdxbxegedabagacad

（三）有了优秀的终端，我们终于可以使用 Shell 了。不过，万里长征才开始了第一步，Shell 也是分门派的，我推荐给大家的是：[终极 Shell——ZSH](http://macshuo.com/?p=676)。

（四）文本编辑器同样是程序员最喜爱的开发工具之一，我个人偏爱 Vim。Vim号称编辑器之神，可以脱离鼠标全键盘操作，良好的插件体系几乎适配各类编程语言，使用起来充满推背的速度感，如果你是个赛车迷，你会喜欢上这款软件的。

推荐阅读 [Vim 系列](http://macshuo.com/?tag=vim)。

其他可选工具：Emacs、TextMate、Sublime Text等。

（五）IDE 是图形化的集成开发工具，具备精准的词法分析、编程提示、调试等功能，功能之繁复用户自知，如果做工业级编程和团队协作的话，推荐使用 IDE。

在这里给大家推荐如下几个工具：

1、Xcode，Mac 上优秀的集成开发工具，几乎所有的 Mac App 和 iOS App 都由此而生，免费软件。无论你是 写 Java 的还是写 Python，用了 Mac 一定要安装 Xcode，为什么？我准备写一篇「更有效率的 XCode」说一下这个事情，当然，这样的内容没那么干，如果各位不同意就算了。

2、JetBrains 系列，产品线丰富，几乎都是精品，Java、Python、Ruby、Php、Objective-C、Web 等一应俱全，收费，还挺贵。

3、Eclipse 系列，通过插件方式几乎支持所有的常用编程语言，免费。

（七）Git 是一款分布式版本控制和软件配置管理软件，类似 SVN 和 CVS，是 Linus 的第二个惊世之作。关于 Linus 和 Git 的故事，我们会在 Linus 系列里描述，这里就不细聊了。

Git 是目前主流的版本管理工具，基于 Git 构建的 Github 网站则是这个星球上最大的开源集散地。还在使用 SVN 和 CVS 的童靴，该换换脑筋了。

回复「git」，你将获得一份Git 简明教程。

图形化的 Git 工具推荐：GitHub、SourceTree。

（八）对于程序员来说，文件比较也属必备工具，OS X 中提供了原生的比较工具 FileMerge，不过这个工具对非 ASCII 内容的文件支持非常不好，推荐 VisualDiffer。VisualDiffer 支持文件和文件夹比较、文件过滤、多重比较模式、颜色标注等，操作简单，响应迅速，实乃程序员居家旅行之必备工具。收费软件，可以直接从 AppStore 下载。

另外，习惯命令行操作的朋友，直接使用 diff 和 vimdiff，也是不错的选择。

（九）xScope 是一款强大的辅助设计工具，可以精确度量屏幕上的 UI 元素，尤其适合全栈工程师。xScope 可以方便的取得屏幕上任意位置的颜色，可以动态智能监测元素边界并显示距离，可以针对移动设备和各种浏览器设定屏幕尺寸，可以设定屏幕辅助线，放大屏幕 等。如果你不想事事求人，xScope是个不错的选择。收费软件。

（十）Pixelmator 号称 Mac 上的精简版 PhotoShop，设计更为人性化，适合非专业人士使用，不是平面设计人员也可以作出非常专业的图像设计。像我这样的老程序员，也开始时不时设计个物件，让团队里的美工 MM 为之侧目。收费软件。

推荐一个Podcast视频教程：http://www.pixelmator.com/tutorials/itunes/

有了这些武器，你将如猛虎加之羽翼而翱翔四海，到时候再说英雄谁是英雄……



## 参考

[brew使用本地安装包 | Charles's Note](https://liyang.pro/brew-install-local-package/)