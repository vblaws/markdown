# 这是普通的一些设置
set ai                    " 自动缩进，与上一行保持一致的自动空格
set ic                    " 在查询模型与匹配模式下忽略大小写
set relativenumber              " 显示相对行号
set nocompatible 		  " 关闭 vi 兼容模式
set showmode              " 文本输入模型下，加亮显示模式指示器
set showcmd               " 在状态栏显示所执行的指令，未完成的指令片段
set warn                  " 长行显示自动折行
set cindent               " 以C/C++模式缩进
set ruler                 " 打开状态栏标尺
set scrolloff=6           " 设置光标离窗口上下6行时窗口自动滚动
set tabstop=4             " 设置Tab长度为4
set shiftwidth=4
set expandtab
set wrap                  " 自动换行显示
set hlsearch              " 设置搜索高亮

syntax enable
syntax on                 " 自动语法高亮
inoremap jk <esc> " 设置jk转换为esc

set cursorcolumn	" 设置列高亮
set cursorline	" 设置行高亮
" 这个是分割线的插件
let mapleader=" " "设置Leader键
"nnoremap <F5> :call Lineandcloumn()<CR>

set guicursor=n-c-v:ver50-ncvCursor " 设置光标样式和大小
" 插件部分
## 插件开始
call plug#begin('~/.vim/plug')

Plug 'preservim/NERDTree' " 这个是目录树插件
Plug 'kien/rainbow_parentheses.vim' " 这个是彩虹括号插件
" --------------- Vim美化 ----------------
" 好看的状态栏
Plug 'vim-airline/vim-airline'
Plug 'vim-airline/vim-airline-themes'

Plug 'mhinz/vim-startify'	" 这个是展示开始界面并且展示一个好看的图标
Plug 'ghifarit53/tokyonight-vim' " 这个是主题插件
" 这个是模糊搜索的插件

Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'

" 这个是markdown预览插件
Plug 'tpope/vim-sensible'
Plug 'junegunn/seoul256.vim'
Plug 'iamcco/mathjax-support-for-mkdp'
Plug 'iamcco/markdown-preview.vim'

call plug#end()
## 插件结束

" 以下是关于插件的配置

"NERDtree
map <F2> : NERDTreeToggle<CR>
" 关于彩虹括号的配置
let g:rbpt_colorpairs = [ ['brown',       'RoyalBlue3'], ['Darkblue',    'SeaGreen3'], ['darkgray',    'DarkOrchid3'], ['darkgreen',   'firebrick3'], ['darkcyan',    'RoyalBlue3'], ['darkred',     'SeaGreen3'], ['darkmagenta', 'DarkOrchid3'], ['brown',       'firebrick3'], ['gray',        'RoyalBlue3'], ['black',       'SeaGreen3'], ['darkmagenta', 'DarkOrchid3'], ['Darkblue',    'firebrick3'], ['darkgreen',   'RoyalBlue3'], ['darkcyan',    'SeaGreen3'], ['darkred',     'DarkOrchid3'], ['red',         'firebrick3'], ]
  let g:rbpt_max = 16
  let g:rbpt_loadcmd_toggle = 0
  au VimEnter * RainbowParenthesesToggle
  au Syntax * RainbowParenthesesLoadRound
  au Syntax * RainbowParenthesesLoadSquare
  au Syntax * RainbowParenthesesLoadBraces
  "ghifarit53/tokyonight-vim " 这个是主题插件配置
  set termguicolors

let g:tokyonight_style = 'night' " available: night, storm
let g:tokyonight_enable_italic = 1

colorscheme tokyonight
" 这个是模糊搜索的插件配置a
"模糊搜索文件
nnoremap <c-p> :Files<CR> 			
nnoremap <c-g> :Ag<CR>
" 普通按键映射
nnoremap <Leader>w :w<CR>
nnoremap <Leader>q :q<CR>
" markdown插件配置
nmap <silent> <Leader>o <Plug>MarkdownPreview        " for normal mode
nmap <silent> <Leader>p <Plug>StopMarkdownPreview    " for normal mode
## 如果喜欢我的配置文件,可以看看我的其他文章,或许会帮到你
[这是我的文章链接](https://www.cnblogs.com/h-xl)
