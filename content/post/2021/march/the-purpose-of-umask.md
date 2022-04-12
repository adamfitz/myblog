+++
title = 'the purpose of umask'
date = '2021-03-15'
author = 'fitzi'
tags = [ 'umask', 'linux' ]
draft = false
+++

When defining default permission for files (0666) and directories (0777) linux has a default set of permissions for each as noted. However if you as a user create a file or directory you will notice that the permissions are not in fact 0666 for a file or 0777 for a directory this is seen below:

```bash {hl_lines=0}
# test file with permissions 0664
$ touch test_file
$ ls -l test_file
-rw-rw-r--. 1 user1 user1 0 Mar 15 06:19 test_file

# test dir with permissions 0775
$ mkdir test_dir
$ ls -l | grep test_dir
drwxrwxr-x.   2 user1 user1       6 Mar 15 06:19 test_dir
```

The reason for this is because it is noted that linux permissions are too permissive. In this case the umask command is used to modify permissions when a file or directory is created.

The way this is determined is by subtracting the umask value from the default permissions, in this example 0666 for files which hen determines the permission value for the file eg:

The default permissions for a file created by a normal user is 0666 minus 0002 equalling 0664

```bash {hl_lines=0}
# default umask value
$ umask
0002

# create a test file
$ touch test_umask_file

# verify the final permissions
$ stat test_umask_file 
  File: test_umask_file
  Size: 0         	Blocks: 0          IO Block: 4096   regular empty file
Device: fd00h/64768d	Inode: 38087484    Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/   user1)   Gid: ( 1000/   user1)
Context: unconfined_u:object_r:user_home_t:s0
Access: 2021-03-15 05:40:17.460682658 -0400
Modify: 2021-03-15 05:40:17.460682658 -0400
Change: 2021-03-15 05:40:17.460682658 -0400
 Birth: -
$
```

The default umask value for normal users is 0002 and the default umask for root users is 0022. The default value can be changed by simply using the umask command for example:

```bash {hl_lines=0}
$ umask 0003
$ umask
0003
```

In order to persist the changes the new umask value must be added to the user profile eg: the .bashrc adding the following line (or whatever value you wish or can change this to):

```bash {hl_lines=0}
umask 0003
```