---
layout: resume
---

### How to zip files and move to the local machine

[1] Find files and copy to a folder\
```
find {folder_path_to_find} -name {file_name_to_find} -exec cp -t {folder_path_to_copy} {} +
```

[2] Zip the folder\
```
zip -r {name_you_want_for_the_zip_folder} {folder_you_want_to_zip}
```

[3] Download in the local server\
```
scp name@rivanna.hpc.virginia.edu:{path_to_the_file} {path_to_the_file_you_want_to_copy; this should include the file name}\
```

When you copy the file from a remote sever, you should use scp commend in your local mahince not in the remote sever!
