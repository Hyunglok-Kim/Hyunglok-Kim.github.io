## &nbsp;
## This page includes simple bash commands/VIM options that I found usefull

### How to zip files and move to the local machine from a HPC server

[1] Find files and copy to a folder <br />
```
find {folder_path_to_find} -name {file_name_to_find} -exec cp -t {folder_path_to_copy} {} +
```

[2] Zip the folder <br />
```
zip -r {name_you_want_for_the_zip_folder} {folder_you_want_to_zip}
```

[3] Download in the local server <br />
```
scp name@rivanna.hpc.virginia.edu:{path_to_the_file} {path_to_the_file_you_want_to_copy; this should include the file name}
```

When you copy the file from a remote sever, you should use scp commend in your local mahince not in the remote sever!


### Useful setting in VIM
[1] vertical split of VIM and scroll two window together
```
:vsp & :Ex & :scrollbind (:help scrollopt)
```

[99] My VIMRC setting
```
:vim ~/.vimrc
syntax on
set exrc
set guicursor=
set nohlsearch
set hidden
set number
set tabstop=4 softtabstop=4
set shiftwidth=4
set expandtab
set noerrorbells
set smartindent
set nowrap
set smartcase
set noswapfile
set nobackup
set undodir=~/.vim/undodir
set undofile
set incsearch
set scrolloff=8
set colorcolumn=80
highlight ColorColumn ctermbg=0 guibg=lightgrey
set relativenumber
set runtimepath^=~/.vim/bundle/ctrlp.vim
```
