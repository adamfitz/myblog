+++
title = 'single PR single commit'
date = '2020-07-10'
author = 'fitzi'
tags = [ 'git' ]
draft = false
+++
When you are using git in a forking workflow, your local commits and pushes will end up on your fork of the repository you are contributing to.

In general a PR should contain single commit (at least the projects that I have worked on) so before you send your PR you will generally want all your commits (changes) in one single commit. In this case there are two options, one is to “squash” all your commits into a single commit or the alternative is to continuously amend each commit to your first one.

This can be accomplished by first adding the changes you want to amend:

```bash {hl_lines=0}
git add $filename
```

After which you can simply use the below command to amend these changes to you previous commit without updating the original commit message.

```bash {hl_lines=0}
git commit --amend --no-edit
```

**NOTE:** Once you amend to a previous commit your subsequent pushes will fail. This is because your local branch is out of sync with the remote branch. To fix this you will be need to add the force flag, be aware that this will overwrite the remote branch with the local changes.

```bash {hl_lines=0}
git push --force
```

Further detailed info can be found here:
https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History