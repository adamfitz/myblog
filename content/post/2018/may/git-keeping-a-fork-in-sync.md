+++
title = 'keeping a fork in sync'
date = '2018-05-15'
author = 'fitzi'
tags = [ 'git' ]
draft = false
+++

Commands to keep a fork in sync with the upstream repo.

To keep it up to date with the original source repo (the one you forked it from):

```bash {hl_lines=0}
git remote add $yourLocalNameForTheUpstream $URLForTheUpstreamRepo
```

To check you set the remote repo as the upstream for your fork:

```bash {hl_lines=0}
git remote -v
```

To check which branch you are working on locally:

```bash {hl_lines=0}
git status
```

Pull changes from the upstream branch:

```bash {hl_lines=0}
git pull $yourLocalNameForTheUpstream  $localBranchName
```

If you accidentally specify the incorrect URL for the remote upstream you forked you can change it by doing the following (as seen when using git remote -v):

```bash {hl_lines=0}
git remote set-url $yourLocalNameForTheUpstream $correctURLForTheUpstreamRepo
```

At this point you have pulled all changes from the remote upstream repo locally to your machine. You then need to push these changes to your remote fork on github:

```bash {hl_lines=0}
git push
```