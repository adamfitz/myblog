+++
title = 'sed exact string match'
date = '2023-03-??'
author = 'fitzi'
tags = [ 'sed', 'linux']
draft = true
+++


# sed basics

I was writing a bash script recently (to install and setup some python scripts from a repo) and as part of this I needed input from the user that I would then write into one of the python scripts which was to be executed.

This is pretty easy to do by using the replace functionality of sed.  For example:

```
sed -i s/
```

The ```-i``` flag is for in place editing.

However I needed to make sure the user input written into to the python file in quotes (as it needed to be declared as a string in the python file), this in itself was a challenge as well as getting the variable expansion correct.

I managed to get this working by using the following combination of single and double quotes along with brace expansion provided by the shell.

```
'"'{$variable_name}'"'
```

In my case I was writing a variable and there were multiple occurance of similar variable naming in the target file I need to match the exact word in order to replace it, I found this can be done with the sed word match feature (`\b`).

Thanks to this [stackoverflow](https://stackoverflow.com/questions/17272968/escaping-characters-in-variables-inside-sed-expression) post for the hints.


One interesting issue I ran into while using `sed` for this aproach is that if the delimiter being used in the sed replacement command is the same as one of the characters entered by the user then your sed command will fail, due to the syntax being incorrect as there is no differentiation between the user input and the delimiter.

