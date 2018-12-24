"1.#git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim 插件管理器
"2.edit .vimrc
"3.run vim and PluginInstall
"4. 自动补全插件：pydiction手动安装
"4.1 cd ~/.vim/bundle, git clone https://github.com/rkulla/pydiction.git
"4.2 cp -r ~/.vim/bundle/pydiction/after/ ~/.vim
"4.3 mkdir -p ~/.vim/tools/pydiction, cp pydiction/complete-dict ~/.vim/tools/pydiction

set nocompatible              " be iMproved, required关闭vi兼容
filetype off                  " required
 
" set the runtime path to include Vundle and initialize
set rtp+=~/.vim/bundle/Vundle.vim
call vundle#begin()
 
" let Vundle manage Vundle, required
Plugin 'VundleVim/Vundle.vim'
Plugin 'davidhalter/jedi-vim' "编程提示插件
Plugin 'vim-syntastic/syntastic' "语法检查
Plugin 'nvie/vim-flake8' "flake8代码风格检查,按F7进行检查
Plugin 'scrooloose/nerdtree' "树形目录,使用Ctrl+n快捷键
Plugin 'jistr/vim-nerdtree-tabs' "vim命令模式下执行：:NERDTree,
"在文件树中:按o键,目录展开或关闭,文件在右侧当前编辑框打开,按i键,文件右侧新编辑框打开
Plugin 'Yggdroot/indentLine' "缩进显示
Plugin 'tell-k/vim-autopep8' "pep8自动格式化代码,使用快捷键F8
Plugin 'vim-scripts/indentpython.vim' "自动缩进插件
Plugin 'kien/ctrlp.vim' "搜索文件,在vim普通模式下搜索ctl+P即可
Plugin 'jiangmiao/auto-pairs' "自动补全括号和引号
Plugin 'ervandew/supertab' "使用supertab插件配合jedi-vim，利用tab键进行自动补全
Plugin 'tmhedberg/SimpylFold' "码折叠，za快捷键
Plugin 'Lokaltog/powerline', {'rtp': 'powerline/bindings/vim/'} "Powerline状态栏
Plugin 'jnurmine/Zenburn' "配色方案
" All of your Plugins must be added before the following line
" 置于call vundle#begin()和call vundle#end()之间，保存配置后在vim中执行
call vundle#end()            " required
filetype plugin indent on    " required
let g:SimpylFold_docstring_preview=1 "希望看到折叠代码的文档字符串

"split navigations
nnoremap <C-J> <C-W><C-J> "切换到下边编辑框bottle
nnoremap <C-K> <C-W><C-K> "切换到上边编辑框top
nnoremap <C-L> <C-W><C-L> "切换到右边编辑框right
nnoremap <C-H> <C-W><C-H> "切换到左边编辑框left

set nocompatible "关闭与vi的兼容模式
set number "显示行号
set nowrap    "不自动折行
set showmatch    "显示匹配的括号
set scrolloff=3        "距离顶部和底部3行"
set encoding=utf-8  "编码
set fenc=utf-8      "编码
set mouse=a        "启用鼠标
set hlsearch        "搜索高亮
syntax on    "语法高亮
let python_highlight_all=1

colorscheme zenburn "配色

"要支持PEP8风格的缩进
au BufNewFile,BufRead *.py
\ set tabstop=4   "tab宽度
\ set softtabstop=4  
\ set shiftwidth=4   
\ set textwidth=79  "行最大宽度
\ set expandtab       "tab替换为空格键
\ set autoindent      "自动缩进
\ set fileformat=unix   "保存文件格式

"对于全栈开发,你可以设置针对每种文件类型设置au命令
au BufNewFile,BufRead *.js, *.html, *.css
\ set tabstop=2
\ set softtabstop=2
\ set shiftwidth=2

"标示不必要的空白字符,这会将多余的空白字符标示出来，很可能会将它们变成红色突出
"au BufRead,BufNewFile *.py,*.pyw,*.c,*.h match BadWhitespace /\s\+$/

"indentLine缩进显示, 没有起作用??????
let g:indentLine_char='┆'
let g:indentLine_enabled = 1
"let g:indentLine_color_term = 239
"映射到ctrl+i键
"map <C-i> :IndentLinesToggle<CR>

" Enable folding折叠
set foldmethod=indent
set foldlevel=99
" Enable folding with the spacebar
nnoremap <space> za

"tab补全,pip install jedi
let g:SuperTabDefaultCompletionType = "context"
let g:jedi#popup_on_dot = 0
"tab补全
let g:pydiction_location = '~/.vim/tools/pydiction/complete-dict'

"隐藏.pyc文件
let NERDTreeIgnore=['\.pyc$', '\~$'] "ignore files in NERDTree

"vim-powerline美化状态
"let g:Powerline_symbols = 'fancy'
let g:Powerline_symbols = 'unicode'

map <F5> :call RunPython()<CR>
func! RunPython()
  exec "W"
  if &filetype == 'python'
    exec "!time python2.7 %"
  endif
endfunc
map <C-n> :NERDTreeToggle<CR>

"F8 pep8, pip install autopep8
autocmd FileType python noremap <buffer> <F8> :call Autopep8()<CR>
