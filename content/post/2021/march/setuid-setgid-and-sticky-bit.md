+++
title = 'setuid, setgid and sticky bit'
date = '2021-03-24'
author = 'fitzi'
tags = [ 'setgid', 'setuid', 'sticky bit', 'linux' ]
draft = false
+++

# **setuid**

The setuid command allows non owner users to run **binary** executable files with the same privileges as the file owner. The setuid can only be set on files and will be ignored if set on a directory.

If an uppercase S shown in the file permissions indicates that the setuid bit has been set but the file is not executable (+x).

The below example shows a lowercase **s** in the user column meaning that the setuid bit is applied to the su binary executable.

```bash {hl_lines=0}
$ ls -l /usr/bin/su
-rwsr-xr-x. 1 root root 50320 Jun 26  2020 /usr/bin/su
```

the setuid bit can be applied using octal notation with the below chmod command, the leading 4 in the octal notation applies the setuid bit, the subsequent zeros do not modify any other rwx permissions on the file:

```bash {hl_lines=0}
$ ls -l
total 0
-rwxrwxr-x. 1 user2000 sgrp 0 Mar 23 16:40 test_file

# apply the setuid bit
$ chmod +4000 test_file 
$ ls -l test_file 
-rwsrwxr-x. 1 user2000 sgrp 0 Mar 23 16:40 test_file
```

You can also change the read, write, execute permissions on the file at the same time using octal notation for example:

```bash {hl_lines=0}
$ ls -l testfile 
-rw-rw-r--. 1 user2000 user2000 0 Mar 24 07:34 testfile

# applying setuid bit and additional permissions
$ chmod 4755 testfile 
$ ls -l testfile 
-rwsr-xr-x. 1 user2000 user2000 0 Mar 24 07:34 testfile
```

The setuid “s” bit can also be set using symbolic notation as seen below:

```bash {hl_lines=0}
$ ls -l test_file 
-rwxrwxr-x. 1 user2000 sgrp 0 Mar 23 17:51 test_file

# apply s bit using rwx notation
$ chmod u+s test_file 
$ ls -l test_file 
-rwsr-xr-x. 1 user2000 sgrp 0 Mar 23 17:51 test_file
```

All the explanation and examples I can find for use of the s bit relates back to enabling users to run executable files as root for example, su. However use of the s bit is not exclusively to inherit the privilege of the root user, it is possible for user 1 to be the owner of a binary executable file, set the s bit and allow user2 the ability to execute the file.

At this point I cannot think of where this would be useful, perhaps when a binary executable file for an application is owned by a service account and another user may require the ability to execute it for troubleshooting purposes maybe. In any event all due care should be taken when implementing this feature as there could well be unintended side effects from a security point of view as you are allowing non owners to effectively impersonate the owner user privileges when executing the binary file.

# **setgid**


The setgid bit is similar to the setuid bit however the permission is applied from the owner group and allows non owners to execute a binary executable file with the same privileges as is assigned to the group members.

If a lowercase **l** appears in the groups execute field (instead of an s) then the execute bit is not set on the file.

Below example shows permissions assigned to the write command, here the **s** bit can be seen in the group column:

```bash {hl_lines=0}
$ ls -l /usr/bin/write
-rwxr-sr-x. 1 root tty 21232 Jun 26  2020 /usr/bin/write
```

The setgid bit can also be applied to directories allowing files and subdirectories to inherit this permission. All files and subdirectories created after the gid is set inherit the group which the directory is a member of. This allows sharing of files and directory contents, without the need to explicitly change file permissions every time a new file or sub directory is created.

```bash {hl_lines=0}
# test directory permissions
$ ls -ld /test_guid/
drwxrws---. 2 root sgrp 6 Mar 22 21:37 /test_guid/

# user1000 creates a file with the following permissions
$ ls -l
total 0
-rw-rw-r--. 1 user1000 sgrp 0 Mar 22 21:45 1_test_setgid

# user2000 is able to modify the file owned by user1000
$ whoami
user2000

$ $ cat /test_guid/1_test_setgid 
test file modification

# user2000 is also able to delete the file because the user is a member of the owner group
$ ls -l
total 0
-rw-rw-r--. 1 user1000 sgrp 0 Mar 22 21:49 1_test_gid

$ rm 1_test_gid 
$ ls -l
total 0
```

Note that in the above, users are members of the sgrp which is the owner group of the directory and the guid bit has been set.

As the owner group for the test file is the sgrp and the group has read and write permissions this means that group members are able to modify and delete files created by other members, even though each individual group member is the owner of the file they both belong to the owner group, giving each member essentially the same privileges.

# **sticky bit**


The sticky bit can be set on public or shared writable directories to prevent files being deleted by normal users who are non owners of the file or directory.

The sticky bit is seen in symbolic notation in the directory listing by the **t** seen in the directory listing column:

```bash {hl_lines=0}
# the t in the other column indicates that the sticky bit is set on this directory
$ ls -ld /tmp
drwxrwxrwt. 19 root root 4096 Mar 23 20:25 /tmp
```

This can be illustrated by creating a file in /tmp as user1000 (the owner) and because the sticky bit is set at the parent directory level, user2000 is unable to delete the file as seen below:

```bash {hl_lines=0}
# file created by user1000
$ pwd
/tmp
$ ls -l sticky_test 
-rw-rw-r--. 1 user1000 user1000 0 Mar 23 20:29 sticky_test

# file cannot be deleted by user2000
$ whoami
user2000
$ rm sticky_test 
rm: remove write-protected regular empty file 'sticky_test'? y
rm: cannot remove 'sticky_test': Operation not permitted
```

However once the sticky bit is removed from the parent directory then user2000 **is** able to delete the file. Even though the owner is user1000 the file exists in a shared directory:

```bash {hl_lines=0}
# remove the sticky bit from parent dir /tmp
[root@server1 /]# chmod o-t /tmp
[root@server1 /]# ls -ld /tmp
drwxrwxrwx. 19 root root 4096 Mar 23 20:30 /tmp

# confirm user2000 is now able to delete file created by user1000
$ whoami
user2000

$ rm /tmp/sticky_test 
$ rm: remove write-protected regular empty file '/tmp/sticky_test'? y
$ ls -l /tmp/sticky_test 
ls: cannot access '/tmp/sticky_test': No such file or directory
```