## &nbsp;
## This page includes simple bash commands/VIM options that I found useful

### How to zip files and move to the local machine from an HPC server

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
When you copy the file from a remote sever, you should use the scp commend in your local machine, not in the remote sever!

[4] View pictures via ssh.

We can use the imagemagick module to preview images from the command line.
```
module load imagemagick
magick display myimage.jpeg
```
We will need to add the -Y flag to our ssh command when we log into a server: e.g. ssh -Y yourcomputingID@rivanna.hpc.virginia.edu.
If we are using a Mac, we will need to make sure we have XQuartz installed, and use the terminal application in XQuartz.

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
[2] SLURM script for LIS (helped by Ruoshi Sun, UVA)
```
Ideally the only things you need to edit are the first 11 lines - resource request and mode selection.
Lines 14-21: Assign variables that will occur more than once. Check existence of paths before moving on. exit will terminate the script and hence the job.
Lines 23-26: Load modules. The “&& ||“ construct is equivalent to “if-else”. If something failed, again the job should exit.
Lines 31-37: Compile LIS and check status.
Lines 39-44: Create symlink to LIS and check status.
Lines 46-58: Edit x and/or y pixels via mode selection. You can always redefine the logic. Note the use of native SLURM environment variables.
Lines 60-80: Export env variables. This should always succeed.
Finally run LIS. I didn’t modify anything here.
Since your job has many steps, it’s important to check the “exit status” of each step before proceeding to the next. So if something fails the job should terminate immediately.

#!/bin/bash
#SBATCH -N 10
#SBATCH --ntasks-per-node=40  # WARNING: do not use -n
#SBATCH -t 1:00:00
#SBATCH -p parallel
#SBATCH -A hydrosense

# Select mode
# -  y: all along y
# - xy: x=#nodes, y=ntasks-per-node
MODE="xy"

# check if LIS directory and config file exist
LISDIR=/home/hk5kp/libs/lis
LISCONFIG="lis.config.noahmp401_ol"
for i in $LISDIR $LISCONFIG; do
    if [ ! -e $i ]; then
        echo "Cannot find $i"
        exit 1
    fi
done

# load modules
MODS="intel/18.0 intelmpi/18.0 grib_api netcdf/4.6.2-hdf5-1.8.22 hdf/4.2.14-fortran hdf5/1.8.22 hdf-eos/2.20-fortran esmf/7.1.0r-hdf5-1.8.22 openjpeg"
module purge
module load $MODS && echo "Loaded modules" || {
    echo "Failed to load modules"
    exit 1
}

# compile
HERE=`pwd`
cd $LISDIR
./compile && echo 0 || {
    echo "LIS failed to compile. Please check ${LISDIR}!"
    exit 1
}

# symlink to LIS - do not copy it
cd $HERE
ln -sf $LISDIR/LIS && echo 1 || {
    echo "Could not create symlink to $LISDIR/LIS"
    exit 1
}

# edit x and y pixels
case $MODE in
xy) sed -i -e "s/Number of processors along x:.*$/Number of processors along x:          $SLURM_NNODES/" \
           -e "s/Number of processors along y:.*$/Number of processors along y:          $SLURM_NTASKS_PER_NODE/" \
           $LISCONFIG
    ;;
y)  sed -i "s/Number of processors along y:.*$/Number of processors along y:          $SLURM_NPROCS/" \
           $LISCONFIG
    ;;
*)  echo "Invalid mode"
    exit 1
    ;;
esac

# export environment variables
#printenv | grep EBROOT
LIBDIR=$HOME/libs
export HDF5_USE_FILE_LOCKING=FALSE \
       LIS_SRC=/home/hk5kp/libs/lis \
       LIS_ARCH=linux_ifort \
       LIS_SPMD=parallel \
       LIS_FC=mpiifort \
       LIS_CC=mpiicc \
       LIS_JASPER=${EBROOTJASPER} \
       LIS_GRIBAPI=${EBROOTGRIB_API} \
       LIS_NETCDF=${EBROOTNETCDF} \
       LIS_HDF4=${EBROOTHDF} \
       LIS_HDF5=${EBROOTHDF5} \
       LIS_HDFEOS=${EBROOTHDFMINEOS} \
       LIS_MODESMF=${EBROOTESMF}/mod/ \
       LIS_LIBESMF=${EBROOTESMF}/lib/ && \
echo "env exported" || {
    echo "Could not export environment variables"
    exit 1
}

pwd
#echo ${LD_LIBRARY_PATH}
echo LIS is running step 2

ulimit -s unlimited
srun ./LIS --file lis.config.noahmp401_ol
echo Job finished step 2
```
