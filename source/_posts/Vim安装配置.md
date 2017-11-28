# Vim安装配置
[Vim配置、插件和使用技巧](http://www.jianshu.com/p/a0b452f8f720)

## 安装 [Vim](http://www.vim.org/)
```
yum install -y vim-common vim-enhanced vim-minimal vim-filesystem
```

## 配置插件管理器 [Vundle](https://github.com/VundleVim/Vundle.vim)
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
let g:airline_symbols.linenr = '␤'
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

## Vim主题
### [Solarized](http://ethanschoonover.com/solarized)
[Solarized Colorscheme for Vim](https://github.com/altercation/vim-colors-solarized)

### Molokai
[Molokai Color Scheme for Vim](https://github.com/tomasr/molokai)