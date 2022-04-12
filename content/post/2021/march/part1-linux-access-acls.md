+++
title = 'Part1: linux Access ACLs'
date = '2021-03-22'
author = 'fitzi'
tags = [ 'ACLs', 'linux' ]
draft = false
+++

ACLs are extended permissions for users or groups in addition to the normal ugo/rwx file permissions. These can be assigned to files (access ACLs) and directories (default ACLs).

ACLs changes can be applied with the **setfacl** command or viewed with the **getfacl** command.

There are a number of arguments that can be used with the setfacl command, a few useful ones can be seen in the table below:

Example: modifying an access ACL:

| flag | description |
|----- | ----------- |
|-m | add to or change (modify) the current ACL
|-x | removes a specific ACL entry eg: removing all permissions for a user
|-b | remove all the currently configured ACLs (careful with this one)


```bash {hl_lines=0}
# initial test file with no extended permissions
$ getfacl -c testfile 
user::rw-
group::rw-
other::r--
```

Add user1 to the ACL with read, write and execute (7) permissions

```bash {hl_lines=0}
$ setfacl -m u:user1:7 testfile 
$ getfacl -c testfile 
user::rw-
user:user1:rwx
group::rw-
mask::rwx
other::r--
```

Add user3 to the ACL with read permissions 6 (r–)

```bash {hl_lines=0}
$ setfacl -m u:user3:r testfile 
$ getfacl -c testfile 
user::rw-
user:user1:rwx
user:user3:r--
group::rw-
mask::rwx
other::r--
```

Remove user1 from the ACL altogether

```bash {hl_lines=0}
$ setfacl -x u:user1 testfile 
$ getfacl -c testfile 
user::rw-
user:user3:r--
group::rw-
mask::rw-
other::r--
```

Remove all ACL entries from file

```bash {hl_lines=0}
$ setfacl -b testfile 
$ getfacl testfile 
user::rw-
group::rw-
other::r--
```