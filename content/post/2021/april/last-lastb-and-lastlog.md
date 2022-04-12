+++
title = 'last, lastb and lastlog'
date = '2021-04-06'
author = 'fitzi'
tags = [ 'last', 'lastb', 'lastlog', 'linux' ]
draft = false
+++

These three tools outlined below are available in linux to audit user logins, server reboots and failed login attempts.

# last


The **last** command displays a list of users and when they last logged into the system. This includes the psuedo user reboot, which will log an entry each time the system is rebooted. Filtering to a specific user (reboot) as shown below will list all the system reboots since the log file was created.

The last command can be run by normal users and does not require root privileges.

```bash {hl_lines=0}
[root@server1 ~]# last reboot
reboot   system boot  4.18.0-240.15.1. Sat Apr  3 20:00   still running
reboot   system boot  4.18.0-240.15.1. Sat Apr  3 19:24 - 19:59  (00:35)
reboot   system boot  4.18.0-240.15.1. Sat Apr  3 07:01 - 19:59  (12:58)
<snip>

wtmp begins Sun Feb 28 12:34:51 2021
```

The last command reads entries from the file **/var/log/wtmp** file:

```bash {hl_lines=0}
[root@server1 ~]# ls -l /var/log/wtmp
-rw-rw-r--. 1 root utmp 91776 Apr  3 20:00 /var/log/wtmp
```

# lastb


The lastb command is the same as the last command except that it will display the list of failed login attempts. The lastb command requires **root** privileges in order to run this command:

```bash {hl_lines=0}
[root@server1 ~]# lastb root
root     ssh:notty    192.168.122.1    Sat Apr  3 20:47 - 20:47  (00:00)
root     ssh:notty    192.168.122.1    Sat Apr  3 20:47 - 20:47  (00:00)
root     ssh:notty    192.168.122.1    Sat Apr  3 20:10 - 20:10  (00:00)
root     ssh:notty    192.168.122.1    Sat Apr  3 20:10 - 20:10  (00:00)

btmp begins Sat Apr  3 20:10:32 2021
```

lastb reads entries from the **/var/log/btmp** file as shown below:

```bash {hl_lines=0}
[root@server1 ~]# ls -l /var/log/btmp
-rw-rw----. 1 root utmp 1920 Apr  3 20:10 /var/log/btmp
```

# lastlog


The *lastlog* command displays the most recent logins for all or a specific user, this includes users with a login shell attached and also system or service accounts. The lastlog command can be executed by normal users.

The file where entries are written and displayed by last log is **/var/log/lastlog**.

```bash {hl_lines=0}
[root@server1 ~]# ls -l /var/log/lastlog
-rw-rw-r--. 1 root utmp 584292 Apr  3 20:00 /var/log/lastlog
```

For example to display the most recent login for a specific user the -u switch can be used as seen below:

```bash {hl_lines=0}
[root@server1 ~]# lastlog -u root
Username         Port     From             Latest
root             pts/1    192.168.122.1    Sat Apr  3 20:00:59 +1100 2021
```

As mentioned above the three files (listed below) that these commands read from are all located in the /var/log directory and are referred to as database files.

- wtmp
- btmp
- lastlog

If you attempt to use cat to read these files you will just see garbage output echoed to stdout likewise if you attempt to open these files with a tool like less you will receive the warning that the file “may be a binary file. See it anyway?”