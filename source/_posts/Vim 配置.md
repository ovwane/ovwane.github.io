date: 2018-05-05 17:00



[Vundle](http://github.com/VundleVim/Vundle.vim)

```shell
$ git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

```
vim ~/.vimrc

set nocompatible              " be iMproved, required
filetype off                  " required

" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
" alternatively, pass a path where Vundle should install plugins
"call vundle#begin('~/some/path/here')

" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'

" The following are examples of different formats supported.
" Keep Plugin commands between vundle#begin/end.
" plugin on GitHub repo
Plugin 'tpope/vim-fugitive'
" plugin from http://vim-scripts.org/vim/scripts.html
" Plugin 'L9'
" Git plugin not hosted on GitHub
Plugin 'git://git.wincent.com/command-t.git'
" git repos on your local machine (i.e. when working on your own plugin)
Plugin 'file:///home/gmarik/path/to/plugin'
" The sparkup vim script is in a subdirectory of this repo called vim.
" Pass the path to set the runtimepath properly.
Plugin 'rstacruz/sparkup', {'rtp': 'vim/'}
" Install L9 and avoid a Naming conflict if you've already installed a
" different version somewhere else.
" Plugin 'ascenator/L9', {'name': 'newL9'}

" All of your Plugins must be added before the following line
call vundle#end()            " required
filetype plugin indent on    " required
" To ignore plugin indent changes, instead use:
"filetype plugin on
"
" Brief help
" :PluginList       - lists configured plugins
" :PluginInstall    - installs plugins; append `!` to update or just :PluginUpdate
" :PluginSearch foo - searches for foo; append `!` to refresh local cache
" :PluginClean      - confirms removal of unused plugins; append `!` to auto-approve removal
"
" see :h vundle for more details or wiki for FAQ
" Put your non-Plugin stuff after this line
```

Install Plugins:

Launch `vim` and run `:PluginInstall`

To install from command line: `vim +PluginInstall +qall`

### 详解

```
   “显示行号
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
```

### 主题配色

[vim-colors-solarized

### Vim插件

[vim-airline](https://github.com/vim-airline/vim-airline)

[**nerdtree**](https://github.com/scrooloose/nerdtree)

[nerdtree-git-plugin](https://github.com/Xuyuanp/nerdtree-git-plugin)

### 参考

[上古神器vim插件：你真的学会用NERDTree了吗？](https://www.jianshu.com/p/3066b3191cb1)