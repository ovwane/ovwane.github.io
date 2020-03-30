---
Title: Vim 使用
date: 2016-08-08 13:20
---

## Vim安装


## 安装 [Vim](http://www.vim.org/)
```
yum install -y vim-common vim-enhanced vim-minimal vim-filesystem
```



## 配置插件管理器 [Vundle](https://github.com/VundleVim/Vundle.vim)

[Vim配置、插件和使用技巧](http://www.jianshu.com/p/a0b452f8f720)



### 安装Vundle

```
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```


[vim中的杀手级插件: vundle](http://zuyunfei.com/2013/04/12/killer-plugin-of-vim-vundle/)



### 配置 ~/.vimrc

```bash
cat > ~/.vimrc << EOF
set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" Color schemes
Plugin 'tomasr/molokai'
Plugin 'altercation/vim-colors-solarized'

" Status Color schemes
Plugin 'vim-airline/vim-airline'
Plugin 'vim-airline/vim-airline-themes'

" plugin on GitHub repo
" Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
" Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
" Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
" Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
" Put your non-Plugin stuff after this line
" solarized start
"let g:solarized_termcolors=256
"let g:solarized_termtrans=1
"let g:solarized_contrast="normal"
"let g:solarized_visibility="normal"
"syntax enable
"set background=dark
"colorscheme solarized
" solarized end
" molokai start
let g:molokai_original = 1
let g:rehash256 = 1
colorscheme molokai
" molokai end
" Airline theme start
let g:airline_theme='molokai'
let g:airline_powerline_fonts = 1
let g:airline#extensions#tabline#enabled = 1
let g:airline#extensions#tabline#buffer_nr_show = 1
let g:airline#extensions#tabline#left_sep = ' '
let g:airline#extensions#tabline#left_alt_sep = '|'
let g:airline#extensions#whitespace#enabled = 0
let g:airline#extensions#whitespace#symbol = '!'
if !exists('g:airline_symbols')
  let g:airline_symbols = {}
endif
" unicode symbols
let g:airline_left_sep = '»'
let g:airline_left_sep = '▶'
let g:airline_right_sep = '«'
let g:airline_right_sep = '◀'
let g:airline_symbols.crypt = '🔒'
let g:airline_symbols.linenr = '☰'
let g:airline_symbols.linenr = '␊'
let g:airline_symbols.linenr = '
'
let g:airline_symbols.linenr = '¶'
let g:airline_symbols.maxlinenr = ''
let g:airline_symbols.maxlinenr = '㏑'
let g:airline_symbols.branch = '⎇'
let g:airline_symbols.paste = 'ρ'
let g:airline_symbols.paste = 'Þ'
let g:airline_symbols.paste = '∥'
let g:airline_symbols.spell = 'Ꞩ'
let g:airline_symbols.notexists = '∄'
let g:airline_symbols.whitespace = 'Ξ'
" Airline theme end
"
set laststatus=2
set t_Co=256
set encoding=utf-8
set syntax enable
set syntax on
set number
"
EOF
```
>“显示行号
set nummber
“语法高亮度显示
syntax on 
“下面两行在进行编写代码时，在格式对起上很有用；
“第一行，vim使用自动对起，也就是把当前行的对起格式应用到下一行；
“第二行，依据上面的对起格式，智能的选择对起方式，对于类似C语言编
“写上很有用
set autoindent
set smartindent
“第一行设置tab键为4个空格，第二行设置当行之间交错时使用4个空格
set tabstop=4
set shiftwidth=4
“设置匹配模式，类似当输入一个左括号时会匹配相应的那个右括号
set showmatch




### 安装插件

```
vim +PluginInstall +qall
```



### 使用方法

```
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
" see :h vundle for more details or wiki for FAQ
```
```
#安装插件 Install Plugins:

Launch vim and run :PluginInstall
```



## Vim插件

### [pathogen](https://github.com/tpope/vim-pathogen)类似于Vundle
```
mkdir -p ~/.vim/autoload ~/.vim/bundle && \
curl -LSso ~/.vim/autoload/pathogen.vim https://tpo.pe/pathogen.vim
```
```
vi .vimrc

execute pathogen#infect()
syntax on
filetype plugin indent on
```

### [vim-airline](https://github.com/vim-airline/vim-airline)

### [vim-airline-themes](https://github.com/vim-airline/vim-airline-themes)

### [nerdtree](https://github.com/scrooloose/nerdtree)

### [nerdtree-git-plugin](https://github.com/Xuyuanp/nerdtree-git-plugin)



## Vim主题

### [Solarized](http://ethanschoonover.com/solarized)
[Solarized Colorscheme for Vim](https://github.com/altercation/vim-colors-solarized)

### Molokai
[Molokai Color Scheme for Vim](https://github.com/tomasr/molokai)



## 编译安装vim

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



## 技巧

### [Vim如何保存需要root权限的文件](https://blog.csdn.net/ch717828/article/details/50843956) , [以普通用户启动的Vim如何保存需要root权限的文件](http://feihu.me/blog/2014/vim-write-read-only-file/)

```
:w !sudo tee %
```



## 参考

[上古神器vim插件：你真的学会用NERDTree了吗？](https://www.jianshu.com/p/3066b3191cb1)

[编译安装vim-8.0 (centos)](https://blog.csdn.net/dinglinuX/article/details/53908313)

[vim 入坑指南（五）插件 Vim-Plug](https://vimzijun.net/2016/09/21/vim-plug/)