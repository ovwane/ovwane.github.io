---
date: 2018-05-05 07:52:28
---

### 安装

[Oh My Zsh](http://ohmyz.sh/)

```shell
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

**主题**

**目前使用的是** ys 

期望使用 agnoster

### Zsh插件

**Clone this repository in oh-my-zsh's plugins directory:**

**[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)**

```Shell
$ git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

```shell
$ git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
```

[zsh-completions](https://github.com/zsh-users/zsh-completions)

```shell
$ git clone https://github.com/zsh-users/zsh-completions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-completions
```

[zsh-history-substring-search](https://github.com/zsh-users/zsh-history-substring-search)

```shell
$ git clone https://github.com/zsh-users/zsh-history-substring-search ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-history-substring-search
```

[zsh-apple-touchbar](https://github.com/zsh-users/zsh-apple-touchbar)

```shell
$ git clone https://github.com/zsh-users/zsh-apple-touchbar ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-apple-touchbar
```



**Add the plugin to the list of plugins for Oh My Zsh to load:**

```Shell
$ vim ~/.zshrc

plugins=(
	zsh-syntax-highlighting
	zsh-autosuggestions
	zsh-completions
	zsh-history-substring-search
	
	zsh-apple-touchbar
	)
```

**Source `~/.zshrc` to take changes into account:**

```
 source ~/.zshrc
```

### 参考

[ITerm2配色方案](https://www.jianshu.com/p/33deff6b8a63)