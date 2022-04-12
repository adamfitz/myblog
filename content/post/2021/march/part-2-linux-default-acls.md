+++
title = 'Part 2: linux Default ACLs'
date = '2021-03-23'
author = 'fitzi'
tags = [ 'ACLs', 'linux' ]
draft = false
+++

Default ACLs can only be applied to directories and their subsequent subdirs and files. Permissions apply recursively to all subdirectories and files within them, however default ACL permissions only apply to files and subdirectories created **AFTER** the default ACL is applied. Existing files and subdirectories do not automatically inherit permissions from the default ACL.

The below example shows the modification of directory permissions with a default ACL for members of the test group named, colab_group:

We start by creating a test directory with the default permissions

```bash {hl_lines=0}
$ mkdir testdir
$ ls -ld testdir
drwxrwxr-x. 2 user1 user1 6 Mar 20 17:08 testdir
```

The test users are members of the group “colab_group”

```bash {hl_lines=0}
$ getent group | grep colab
colab_group:x:9003:user3,user4,user1
```

Create a test file and view permissions before adding default ACL permissions:

```bash {hl_lines=0}
[user1@server1 testdir]$ pwd
/tmp/testdir

[user1@server1 testdir]$ ls -l
total 0
-rw-rw-r--. 1 user1 user1 0 Mar 20 17:18 1_test_perms

# current file permissions
$ getfacl 1_test_perms 
# file: 1_test_perms
# owner: user1
# group: user1
user::rw-
group::rw-
other::r--

# current sub directory permissions
$ getfacl 1_subdir/
# file: 1_subdir/
# owner: user1
# group: user1
user::rwx
group::rwx
other::r-x

# current parent directory permissions
$ getfacl testdir
# file: testdir
# owner: user1
# group: user1
user::rwx
group::rwx
other::r-x
```

Apply the default ACL permissions with rwx (7) access to the parent testdir:

```bash {hl_lines=0}
$ setfacl -m d:g:colab_group:rwx testdir
$ getfacl testdir
# file: testdir
# owner: user1
# group: user1
user::rwx
group::rwx
other::r-x
default:user::rwx
default:group::rwx
default:group:colab_group:rwx
default:mask::rwx
default:other::r-x
```

After the default ACL is applied to the parent directory, we verify that the plus symbol ‘+’ is seen in the directory permissions listing, indicating that extended permissions exist and an ACL is applied:

```bash {hl_lines=0}
$ ls -ld testdir
drwxrwxr-x+ 3 user1 user1 42 Mar 20 17:23 testdir
```

Checking the permissions of the existing file and sub directory shows that the permissions have not changed:

```bash {hl_lines=0}
>$ ls -l
total 0
drwxrwxr-x. 2 user1 user1 6 Mar 20 17:23 1_subdir
-rw-rw-r--. 1 user1 user1 0 Mar 20 17:18 1_test_perms
```

Creating another file and directory now that the default ACL has been applied to the parent directory shows the extended permissions/default ACL are now applied as these have been created after the ACL was applied to the parent directory and new files and directories are now inheriting the ACL permissions.

The file **2_test_perms** and the directory **2_subdir** now show they have extended permissions applied:

```bash {hl_lines=0}
$ ls -l
total 0
drwxrwxr-x. 2 user1 user1 6 Mar 20 17:23 1_subdir
-rw-rw-r--. 1 user1 user1 0 Mar 20 17:18 1_test_perms
drwxrwxr-x+ 2 user1 user1 6 Mar 20 17:32 2_subdir
-rw-rw-r--+ 1 user1 user1 0 Mar 20 17:31 2_test_perms
```

Swapping to another user3 who is a member of the group “colab\_group” we can see user3 has permissions to modify or create files with the extended permissions but no rights to modify the initial files created before the default ACL was applied to the parent directory

```bash {hl_lines=0}
$ whoami
user3

$ pwd
/tmp/testdir

$ ls -l
total 0
drwxrwxr-x. 2 user1 user1 6 Mar 21 08:23 1_subdir
-rw-rw-r--. 1 user1 user1 0 Mar 21 08:18 1_test_perms
drwxrwxr-x+ 2 user1 user1 6 Mar 21 08:32 2_subdir
-rw-rw-r--+ 1 user1 user1 0 Mar 21 08:31 2_test_perms

# user 3 is allowed to edit the test file
$ cat 2_test_perms 
user3 can edit the test file

# while user 3 CAN edit the 2_test_perms file (with the extended permissions) user3 CANNOT remove the file because user3 is not the owner
$ ls -l 2_test_perms
-rw-rw-r--+ 1 user1 user1 29 Mar 21 09:04 2_test_perms

$ rm 2_test_perms 
rm: cannot remove '2_test_perms': Permission denied

$ getfacl 2_test_perms 
# file: 2_test_perms
# owner: user1
# group: user1
user::rw-
group::rwx			#effective:rw-
group:colab_group:rwx		#effective:rw-
mask::rw-
other::r--

# user 3 has permissions to enter and create files in the sub dir with the extended permissions assigned
$ pwd
/tmp/testdir/2_subdir

$ touch test_subdir_file1
[user3@server1 2_subdir]$ ls -l
total 0
-rw-rw-r--+ 1 user3 user3 0 Mar 21 09:05 test_subdir_file1

# user 3 can enter the subdir WITHOUT extended permissions assigned but CANNOT create files
$ cd 1_subdir/

$ pwd
/tmp/testdir/1_subdir

$ touch test_subdir_file2
touch: cannot touch 'test_subdir_file2': Permission denied
```

This post illustrates how using default ACL applied to a directory can be used to give group members access to create and modify files with a directory and subdirectories.