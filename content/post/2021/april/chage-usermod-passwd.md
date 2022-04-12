+++
title = 'chage, usermod, passwd'
date = '2021-04-27'
author = 'fitzi'
tags = [ 'chage', 'passwd', 'usermod', 'linux' ]
draft = false
+++
\
Both the passwd and usermod command can be used to lock or unlock a user account as well as modifying password ageing information for the account. The chage command however is only able to modify password ageing information, but does have the ability to expire a user account.

# chage


Only has the capability to modify a user accounts password expiry information.

```bash {hl_lines=0}
[root@server1 ~]# chage -h
Usage: chage [options] LOGIN

Options:
  -d, --lastday LAST_DAY        set date of last password change to LAST_DAY
  -E, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -h, --help                    display this help message and exit
  -I, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -l, --list                    show account aging information
  -m, --mindays MIN_DAYS        set minimum number of days before password
                                change to MIN_DAYS
  -M, --maxdays MAX_DAYS        set maximum number of days before password
                                change to MAX_DAYS
  -R, --root CHROOT_DIR         directory to chroot into
  -W, --warndays WARN_DAYS      set expiration warning days to WARN_DAYS
```

# passwd


The passwd command is used to set or modify a users password in addition to being able to modify user account attributes and lock or unlock a users account.

Locking an account will add an “!” to the beginning of the password field in /etc/shadow the corresponding unlock flag will remove this “!” from the field.

Locking a user account with the passwd command is accomplished with the -l flag:

```bash {hl_lines=0}
[root@server1 ~]# passwd -l user10
Locking password for user user10.
passwd: Success

[root@server1 ~]# grep user10 /etc/shadow
user10:!!$6$xpkPmLMWCP5NFp/h$VYfghUJGyeRt64nQFaTLNLctrryrawaMeEBc99SpsjJv0U6rr3.nLyDNfbbegB3DtIylnnB1dH.RqQ6IAJHT7.:18743:0:99999:7:::
```

Unlocking a user account with passwd command with the -u flag, shown below:

```bash {hl_lines=0}
[root@server1 ~]# passwd -u user10
Unlocking password for user user10.
passwd: Success

[root@server1 ~]# grep user10 /etc/shadow
user10:$6$xpkPmLMWCP5NFp/h$VYfghUJGyeRt64nQFaTLNLctrryrawaMeEBc99SpsjJv0U6rr3.nLyDNfbbegB3DtIylnnB1dH.RqQ6IAJHT7.:18743:0:99999:7:::
```

Further useful information is found in the man files or with the help command:

```bash {hl_lines=0}
[root@server1 ~]# passwd --help
Usage: passwd [OPTION...] <accountName>
  -k, --keep-tokens       keep non-expired authentication tokens
  -d, --delete            delete the password for the named account (root only); also removes password lock if any
  -l, --lock              lock the password for the named account (root only)
  -u, --unlock            unlock the password for the named account (root only)
  -e, --expire            expire the password for the named account (root only)
  -f, --force             force operation
  -x, --maximum=DAYS      maximum password lifetime (root only)
  -n, --minimum=DAYS      minimum password lifetime (root only)
  -w, --warning=DAYS      number of days warning users receives before password expiration (root only)
  -i, --inactive=DAYS     number of days after password expiration when an account becomes disabled (root only)
  -S, --status            report password status on the named account (root only)
      --stdin             read new tokens from stdin (root only)

Help options:
  -?, --help              Show this help message
      --usage             Display brief usage message
```

# usermod


The usermod command is for modifying user account attributes but it may also be used to lock or unlock the user account by using the flags -L and -U respectively as seen below:

Locking a user account with the usermod command:

```bash {hl_lines=0}
[root@server1 ~]# usermod -L user10
[root@server1 ~]# grep user10 /etc/shadow
user10:!$6$xpkPmLMWCP5NFp/h$VYfghUJGyeRt64nQFaTLNLctrryrawaMeEBc99SpsjJv0U6rr3.nLyDNfbbegB3DtIylnnB1dH.RqQ6IAJHT7.:18743:0:99999:7:::
```

usermod can unlock an account with the -U flag:

```bash {hl_lines=0}
[root@server1 ~]# usermod -U user10
[root@server1 ~]# grep user10 /etc/shadow
user10:$6$xpkPmLMWCP5NFp/h$VYfghUJGyeRt64nQFaTLNLctrryrawaMeEBc99SpsjJv0U6rr3.nLyDNfbbegB3DtIylnnB1dH.RqQ6IAJHT7.:18743:0:99999:7:::
```

More flags are available for the usermod command and can be discovered via the man pages or with the help command:

```bash {hl_lines=0}
[root@server1 ~]# usermod --help
Usage: usermod [options] LOGIN

Options:
  -c, --comment COMMENT         new value of the GECOS field
  -d, --home HOME_DIR           new home directory for the user account
  -e, --expiredate EXPIRE_DATE  set account expiration date to EXPIRE_DATE
  -f, --inactive INACTIVE       set password inactive after expiration
                                to INACTIVE
  -g, --gid GROUP               force use GROUP as new primary group
  -G, --groups GROUPS           new list of supplementary GROUPS
  -a, --append                  append the user to the supplemental GROUPS
                                mentioned by the -G option without removing
                                the user from other groups
  -h, --help                    display this help message and exit
  -l, --login NEW_LOGIN         new value of the login name
  -L, --lock                    lock the user account
  -m, --move-home               move contents of the home directory to the
                                new location (use only with -d)
  -o, --non-unique              allow using duplicate (non-unique) UID
  -p, --password PASSWORD       use encrypted password for the new password
  -R, --root CHROOT_DIR         directory to chroot into
  -P, --prefix PREFIX_DIR       prefix directory where are located the /etc/* files
  -s, --shell SHELL             new login shell for the user account
  -u, --uid UID                 new UID for the user account
  -U, --unlock                  unlock the user account
  -v, --add-subuids FIRST-LAST  add range of subordinate uids
  -V, --del-subuids FIRST-LAST  remove range of subordinate uids
  -w, --add-subgids FIRST-LAST  add range of subordinate gids
  -W, --del-subgids FIRST-LAST  remove range of subordinate gids
  -Z, --selinux-user SEUSER     new SELinux user mapping for the user account
```