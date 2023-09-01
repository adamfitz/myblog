+++
title = 'sed exact string match'
date = '2023-03-??'
author = 'fitzi'
tags = [ 'sed', 'linux']
draft = true
+++


# sed basics

I was writing a bash script recently (to install and setup some python scripts from a repo) and as part of this I needed input from the user that I then needed to write into one of the python scripts to be ecxecuted.

This is pretty easy to do by using the replace functionality of sed.

However I need to have the user input written out to the python file in quotes (as it needed to be declared as a string in the python file), this in itself was a challenge as well as getting the variable expansion correct.

I managed to get this working by usinbg the following combination of single and dobule quotes and brace expansion(?) provided by the shell.

```
'"'{$variable_name}'"'
```

One interesting issue I ran into with this aproach is that if the delimiter you are using in the sed replacement command is the same as one of the characters entered by the user then your sed command will fail, because the syntax will be incorrect as it does not know.

Since I was writing a variable and there were multiple occurance of similar variable naming in the target file I need to match the exact word in order to replace it.  This can be done with the sed word match feature (\b).

