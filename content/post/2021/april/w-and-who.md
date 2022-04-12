+++
title = 'w and who'
date = '2021-04-07'
author = 'fitzi'
tags = [ 'w', 'who', 'linux' ]
draft = false
+++

# **who**


The who command displays the users (who) are currently logged into the system, the time the user logged in and the source (remote) IP or tty line the user is connected via.

```bash {hl_lines=0}
[user1@server1 ~]$ who
root     pts/0        2021-04-06 06:05 (192.168.122.1)
user1    pts/1        2021-04-06 06:13 (192.168.122.1)
root     tty2         2021-04-06 06:12 (tty2)
```

# **w**


The w command displays the currently logged in users and what they are doing. For example the w command shows the tty name, login time, idle time, and the command line of the current process.

In addtion the w command displays the **JCPU**, which is the time used by all processes attached to the tty session and the **PCPU** time is the time used by the current process in the what field.

Below we can see the first root users current process is *bash*, user1’s current process is the *w*  command and the second root users current process is */usr/libexec/gsd-disk-utility-notify*.

In addition we can see the FROM field lists the remote IP for the first two users and tty2 for the second root user which also indicates that the second root user is logged into the servers console.

```bash {hl_lines=0}
[user1@server1 ~]$ w
 06:14:10 up 9 min,  3 users,  load average: 0.31, 0.18, 0.10
USER     TTY      FROM             LOGIN@   IDLE   JCPU   PCPU WHAT
root     pts/0    192.168.122.1    06:05    5:35   0.11s  0.03s -bash
user1    pts/1    192.168.122.1    06:13    1.00s  0.01s  0.00s w
root     tty2     tty2             06:12    9:11   6.34s  0.00s /usr/libexec/gsd-disk-utility-notify
```