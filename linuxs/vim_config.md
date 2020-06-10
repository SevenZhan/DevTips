install vim

    sudo apt remove vim-tiny
    sudo apt update
    sudo apt install vim


make config file

    touch ~/.vimrc


configures


    " python indentation
    au BufNewFile,BufRead *.py
        \ set tabstop=4 |
        \ set softtabstop=4 |
        \ set shiftwidth=4 |
        \ set textwidth=79 |
        \ set expandtab |
        \ set autoindent |
        \ set fileformat=unix


    " python with anaconda
    let g:ycm_python_binary_path='~/anaconda3/bin/python'


    " Flagging Unnecessary Whitespace
    highlight BadWhitespace ctermbg=red guibg=darkred
    au BufRead,BufNewFile *.py,*.pyw,*.c,*.h match BadWhitespace /\s\+$/


    " UTF-8
    set encoding=utf-8


    " beautify python code
    let python_highlight_all=1
    syntax on


    " line number
    set nu