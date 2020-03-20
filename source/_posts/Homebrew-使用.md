---
title: Homebrew 使用
date: 2017-10-20 14:19:51
---

# [Homebrew](https://brew.sh/)

## 安装

```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
```

> 路径：/usr/local/Cellar



## [Homebrew Formulae](https://formulae.brew.sh/)



## Shell 自动补全

[Homebrew Shell Completion](https://docs.brew.sh/Shell-Completion)

~/.zshrc

```
if type brew &>/dev/null; then
  FPATH=$(brew --prefix)/share/zsh/site-functions:$FPATH

  autoload -Uz compinit
  compinit
fi
```



## 编辑器插件

[ Editor plugins](https://docs.brew.sh/Tips-N'-Tricks#editor-plugins)



## 软件管理

更新所有软件

```
brew upgrade
```

```
brew cask upgrade
```



清理软件备份文件

```
brew cleanup
```



安装软件

```
brew install make
```

```
brew cask install java8 goland clion postman
```



列出更新软件列表

```
brew cask outdated
```



更新指定软件

```
brew cask install --force $(brew cask outdated | awk '{print $1}' | xargs)
```



## 命令速查

[Homebrew cheatsheet](https://devhints.io/homebrew)



## 参考

 [「Mac」Homebrew：Mac 必裝的套件管理工具 | Victor Hung's Diary](https://diary.taskinghouse.com/posts/4766365-homebrew-essential-mac-suite-management-tools/) 

