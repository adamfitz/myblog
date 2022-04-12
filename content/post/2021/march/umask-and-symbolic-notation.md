+++
title = 'umask and symbolic notation'
date = '2021-03-17'
author = 'fitzi'
tags = [ 'umask', 'linux' ]
draft = false
+++

While studying for the RHCSA exam I came across a question with the requirement to set the umask value using symbolic notation. This seems like a fairly simple task but I found it had one confusing quirk in that when using symbolic notation.

Taking a look at the [umask(1p) page](https://man7.org/linux/man-pages/man1/umask.1p.html) under the operands section it is mentioned:

>In a symbolic_mode value, the permissions op characters '+' and '-' shall be interpreted relative to the current file mode creation mask; '+' shall cause the bits for the indicated permissions to be cleared in the mask; '-' shall cause the bits for the indicated permissions to be set in the mask.sk.


This seems to indicate addition operator is ‘-‘ and subtraction operator becomes ‘+’ due to the nature of umask (initial permission minus the umask) when using symbolic notation.

Without knowing or understanding this “quirk” beforehand it becomes a little confusing if you are attempting to change the default umask using symbolic notation.

For example: Set the umask value to 0035 using symbolic notation. One would think that the following would be the correct way to accomplish this:

```bash {hl_lines=0}
$ umask u=,g=wx,o=rx
```

The thinking behind this being owner has zero permissions, group set to write and execute and other set to read and execute, however checking the value after applying this value shows:

```bash {hl_lines=0}
$ umask
0742
```

Which at first seems to make no sense, the requested value is 0035, ignoring the leading zero this would equate to the following symbolic values above, zero bits in the owner column (—), 3 bits in the group column (-wx) and 5 bits in the other column (r-x).

However this is not the case, referring to the above link where it is indicated that the umask permissions will be interpreted in relation to the current file mode.

An easy way to remember this, is when setting the umask value subtract the required decimal number from 7 and then calculate the symbolic notation from the result, for example: to correctly set the value of 0035, you would be looking for the symbolic equivalent of 0742 which is:

```bash {hl_lines=0}
$ umask u=rwx,g=r,o=w
$ umask
0035
```