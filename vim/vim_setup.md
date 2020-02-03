## Vim Setup

#### Install basic packages
- Start by installing Vim-Plug:
 - `curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim`
- in the file `~/.vimrc` file, add the following:
```
" Plugins placed between begin and end calls
" Each plug is the user and repo name
call plug#begin('~/.vim/plugged')
Plug 'joshdick/onedark.vim'
Plug 'preservim/nerdtree'
Plug 'ctrlpvim/ctrlp.vim'
Plug 'pangloss/vim-javascript'
call plug#end()

" Set the colorscheme to be used
silent! colorscheme onedark

" Set behavior of backspace button
set backspace=2

" Set the tab and space width in code files
set shiftwidth=2
set tabstop=2

" Set the line numbers to be relative
set relativenumber
set number

" Search file as you type the name
set incsearch

" Set mouse scroll to active
set mouse=a

" Set yank to system clipboard by default
set clipboard=unnamed

" Show hidden files/directories by default
let NERDTreeShowHidden=1

" Aliases for long commands
cabbrev t NERDTree
cabbrev tf NERDTreeFind
cabbrev pi PlugInstall

```

#### Add bash_profile alias to config file
Add `alias vimconfig='vim ~/.vimrc'` to bash_profile aliases


***
[Table of Contents](../README.md)
