```shell
yum -y install zsh
```

查看shell列表

```shell
cat /etc/shells 
```

切换shell为zsh

```shell
chsh -s /bin/zsh
```

```
sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"
```

插件

插件管理工具

antigen、zgen、prezto、zplug

```shell
git clone --recursive https://github.com/sorin-ionescu/prezto.git "${ZDOTDIR:-$HOME}/.zprezto"

setopt EXTENDED_GLOB
for rcfile in "${ZDOTDIR:-$HOME}"/.zprezto/runcoms/^README.md(.N); do
  ln -s "$rcfile" "${ZDOTDIR:-$HOME}/.${rcfile:t}"
done
```



# **zsh-syntax-highlighting**

```shell
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
```

```
 vim .zshrc
 
 plugins=( [plugins...] zsh-syntax-highlighting)
```



# **zsh-completions**

```shell
git clone https://github.com/zsh-users/zsh-completions ~/.oh-my-zsh/custom/plugins/zsh-completions
```

```
vim .zshrc

plugins=(… zsh-completions)
autoload -U compinit && compinit
```



```
source ~/.zshrc
```

参考

[oh-my-zsh,让你的终端从未这么爽过](https://www.jianshu.com/p/d194d29e488c)

