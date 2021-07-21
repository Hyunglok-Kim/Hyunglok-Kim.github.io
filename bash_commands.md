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

[4] View pictures via ssh.

We can use the imagemagick module to preview images from the command line.
```
module load imagemagick
magick display myimage.jpeg
```
We will need to add the -Y flag to our ssh command when we log into a server: e.g. ssh -Y yourcomputingID@rivanna.hpc.virginia.edu.
If we are using a Mac, we will need to make sure we have XQuartz installed, and use the terminal application in XQuartz.

### SLURM settings
[1] standard partition with largemem
```
#!/bin/bash
#SBATCH --nodes = 1
#SBATCH --mem-per-cpu=36000
#SBATCH --time=72:00:00
#SBATCH --partition=standard
#SBATCh --account=hydroxxxx
```

### Useful settings in VIM
[1] vertical split of VIM and scroll two window together
```
:vsp & :Ex & :set scrollbind (:help scrollopt)
:set noscrollbind
```

[2] :resize command or its shortcut :res to change the height of the window
```
:vertical resize 80
```

[3] Horizontal Speed
```
f*        : jumps to character
t*        : jumps to behind character
F* and T* : jump backwards trhough results
; and ,   : jump forward or backwards through results
x         : delete a character
s         : delete a character and entre insert mode
cw (ce)   : delete a word and enter insert mode
combos: dt), vf), vt), yt), ct), etc
D         : delete rest of line
C         : delete rest of line and enter insert mode
```

[4] Vertical Domination
```
gg           : jump to the top and "G" to jump to the bottom
:100 or 100G : jump to line 100
12(j or k)   : jump to 12 down or up
{ and }      : hop to back and forth between empty lines
Ctrl + u (d) : jump half-page up and half-page down
%            : jump to matching pair of curly braces, brackets, parentheses
*i{          : do command * on inside of curlly brace e.g., di{, d2i{, ci{
*ip          : do command * on paragraph e.g., cip, vip
*a[          : do command * on inside of braces including braces e.g., da{
*diw         : delete word if you're in the middle of word
```

[5] File Movements, Buffers, Splits
```
:e <folder> to opden finder
Ctrl + p      : open plug-in finder (fzf recommended)
Ctrl + ^      : jump between last two files
Ctrl + o or i : jump backwards and forwards through history
m and `       : set a mark and go to a mark
mH            : set a global mark
Ctrl + w      : then o to close all but current buffer
Ctrl + w      : then = to equally spread splits
Ctrl + w      : then r to rotate buffers
Ctrl + w      : then H to swith from horizontal to vertical split
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
