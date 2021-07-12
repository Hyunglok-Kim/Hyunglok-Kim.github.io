## _
## This page includes simple bash command lines that I found usefull in Linux

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
