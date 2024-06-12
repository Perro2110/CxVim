# CxVim
Installazione minima per avere un buon ide di <img src="https://cdn.icon-icons.com/icons2/2415/PNG/512/c_original_logo_icon_146611.png" alt="java" width="20" height="20"/> attraverso piccole modifiche su <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4f/Icon-Vim.svg/1024px-Icon-Vim.svg.png" alt="java" width="20" height="20"/>
```.sh
#!/bin/bash

# Aggiorna i pacchetti/installa vim/dipendenze necessarie
sudo apt update
sudo apt install -y vim curl build-essential cmake python3-dev

# Installa vim-plug
curl -fLo ~/.vim/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim

# Crea il file .vimrc con la configurazione necessaria
cat <<EOF > ~/.vimrc
" Initialize vim-plug
call plug#begin('~/.vim/plugged')

" Autocompletion plugin
Plug 'Valloric/YouCompleteMe'

" Syntax highlighting and other useful plugins
Plug 'scrooloose/nerdtree'
Plug 'vim-airline/vim-airline'
Plug 'sheerun/vim-polyglot'

call plug#end()

" Enable syntax highlighting
syntax on

" Enable NERDTree
map <C-n> :NERDTreeToggle<CR>

" Enable vim-airline
let g:airline#extensions#tabline#enabled = 1

" Autocompletion settings
let g:ycm_global_ycm_extra_conf = '~/.vim/.ycm_extra_conf.py'
let g:ycm_confirm_extra_conf = 0

" Misc settings
set number
set relativenumber
set tabstop=4
set shiftwidth=4
set expandtab
EOF

# Installa plugin di Vim
vim +PlugInstall +qall

# Compila YouCompleteMe
cd ~/.vim/plugged/YouCompleteMe
python3 install.py --clangd-completer

# Crea il file di configurazione per YouCompleteMe
cat <<EOF > ~/.vim/.ycm_extra_conf.py
def Settings( **kwargs ):
    return {
        'flags': [ '-x', 'c', '-Wall', '-Wextra', '-Werror', '-std=c11', '-I/usr/include', '-I/usr/local/include' ],
    }
EOF

echo "Configurazione completata. Apri Vim e inizia a scrivere codice C!"
```
Salva questo script in un file, ad esempio setup_vim.sh, e rendilo eseguibile:
```.sh
chmod +x setup_vim.sh
```
esegui poi lo script 
```.sh
./setup_vim.sh
```
