---
title: Vim 配置vim-plug
date: 2018-09-01 20:48:13
tags:
---

## 安装vim

1.检查vim旧版本，若已存在，将其卸载。

```shell
$ vim

$ yum remove vim* -y
```

2.安装终端字符处理库nucrses

```shell
$ yum install ncurses-devel -y
```

编译安装

```shell
$ cd /usr/local/src/

$ wget https://codeload.github.com/vim/vim/tar.gz/v8.0.0134

$ tar zxf v8.0.0134

$ cd vim-8.0.0134/

$ ./configure --prefix=/usr/local/vim8

$ make && make install

$ echo $?
```

安装成功后，通过/usr/local/vim8/bin/vim运行vim命令。将vim命令路径添加到系统PATH环境变量，就可以直接运行vim了。本文不修改/etc/profile文件，通过添加脚本到/etc/profile.d/实现。

```shell
/usr/local/vim8/bin/vim /etc/profile.d/path.sh
添加以下内容：
#!/bin/bash
export PATH=$PATH:/usr/local/vim8/bin/

$ source /etc/profile.d/path.sh
$ vim
```

## [vim-plug](https://github.com/junegunn/vim-plug)

安装

```shell
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

配置

.vimrc

```shell
cat > ~/.vimrc <<EOF
" Specify a directory for plugins
" - For Neovim: ~/.local/share/nvim/plugged
" - Avoid using standard Vim directory names like 'plugin'
call plug#begin('~/.vim/plugged')

" Make sure you use single quotes

" Shorthand notation; fetches https://github.com/junegunn/vim-easy-align
" Plug 'junegunn/vim-easy-align'

" Any valid git URL is allowed
" Plug 'https://github.com/junegunn/vim-github-dashboard.git'

" Multiple Plug commands can be written in a single line using | separators
" Plug 'SirVer/ultisnips' | Plug 'honza/vim-snippets'

" On-demand loading
" Plug 'scrooloose/nerdtree', { 'on':  'NERDTreeToggle' }
" Plug 'tpope/vim-fireplace', { 'for': 'clojure' }

" Using a non-master branch
" Plug 'rdnetto/YCM-Generator', { 'branch': 'stable' }

" Using a tagged release; wildcard allowed (requires git 1.9.2 or above)
Plug 'fatih/vim-go'  ", { 'tag': '*' }

""
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'

" Plugin options
" Plug 'nsf/gocode', { 'tag': 'v.20150303', 'rtp': 'vim' }

" Plugin outside ~/.vim/plugged with post-update hook
" Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --all' }

" Unmanaged plugin (manually installed and updated)
" Plug '~/my-prototype-plugin'

" Initialize plugin system
call plug#end()
EOF
```

安装插件

```shell
vim esc模式下
:PlugInstall
```

[vim-airline](https://github.com/vim-airline/vim-airline)



## 参考

[编译安装vim-8.0 (centos)](https://blog.csdn.net/dinglinuX/article/details/53908313)

[vim 入坑指南（五）插件 Vim-Plug](https://vimzijun.net/2016/09/21/vim-plug/)