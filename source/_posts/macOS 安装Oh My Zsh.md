title: macOS 安装Oh My Zsh
date: 2017-10-20 12:02:00
categories:
- 技术
- macOS
tags:
- macOS
- Oh My Zsh
---
macOS 安装[Oh My Zsh](http://ohmyz.sh/)
macOS 10.12.6
macOS 10.13

## macOS 安装 [Oh My Zsh](https://github.com/robbyrussell/oh-my-zsh)
```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

## 安装插件
### 安装[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting/blob/master/INSTALL.md)
```
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
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
### ~/.zshrc文件
```
# 插件
~/.zshrc
plugins=(git autojump osx colored-man-pages brew zsh-syntax-highlighting)

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

### iTerm2 配色方案
下载[Solarized](http://ethanschoonover.com/solarized)配色

***
打开Preferenced->Profiles
新建profile为 ovwane。

打开iTerm2的偏好设定
Profiles->Colors
点开右下角的Color Presets 选择Import... 直接加载iterm2-colors-solarized/Solarized Dark.itermcolors配色方案就可以了，重启iTerm2就可以看到效果。
***