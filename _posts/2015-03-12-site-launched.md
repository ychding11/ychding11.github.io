---
layout: post
title: "Launches Site"
date: 2015-03-11
---

Today, I launches my tech blog site on github. This post gives a summary about 
how to setup jekyll environment and git operation.

## configure external git difftool
---

[diffmerge](http://sourcegear.com/diffmerge/index.html) is a good option. It is
platform independent and free of charge. Steps to configure git difftool.

1. download and install diffmerge [download link](http://sourcegear.com/diffmerge/index.html).
2. run git command `git config --global diff.tool sgdm` add sgdm as git difftool.
3. run git command `git config --global difftool.sgdm.cmd 'sgdm "$LOCAL" "$REMOTE"'` configure sgdm cmd.

Now new git difftool should be ready. `git difftool master xxx.txt` run difftool to compare xxx.txt in current 
branch with that in master branch. `git config --global --get-all diff.tool` gets all available difftools for 
current user.

Steps to configure git mergetool

1. `git config --global merge.tool sgdm`
2. `git config --global mergetool.sgdm.cmd 'sgdm --merge --result="$MERGED" "$LOCAL" "$BASE" "$REMOTE"'`
3. `git config --global mergetool.sgdm.trustExitCode true`

After ready, when conflicts occurs, run `git mergetool`, a GUI tool will be opened. Above all, you also can
edit git config file directly by command `git config --global -e`.

## branch & remote operation
---

1. `git branch -a` display all branchs including remote and local ones.
2. `git checkout remote/branch-name`
3. `git checkout -b branch-name`

- `git branch -vv` check upstream branch tracked by local branch.
- `git push --set-upstream server branch` push and set up-stream for local branch.
- `git remote -vv`, `git remote add`, `git remote remove`.

## pack two git commits into a single one
---

*git rebase -i* can be used as a tool to compact two commits into one. It is an git interactive operation.
There are commit log: commit A, commit B, commit C. If we combine A and B as a single one, for example, 
commit A&B. Use git rebase -i "commit C id" to let git begins interactive mode. A commit message will appear in
editor, modify commit B from pick to squash in editor and save. After git command finished, commit A and commit B
will combined as a new one.

## How to use math formula in Github page
---

- jekyll [math support](https://jekyllrb.com/docs/extras/#math-support)
- visit [LaTex wiki](https://en.wikibooks.org/wiki/LaTeX/Mathematics) for mathematics.
- sample [page](http://gastonsanchez.com/visually-enforced/opinion/2014/02/16/Mathjax-with-jekyll/)

## p4 command
---

- *p4 have file*, check which revision of file in workspace.
- *p4 where file*, output depot syntax of file, no checking.
- *p4 filelog -l file*, check file history with long format.
- *p4 describle cl*, display info of specifing change list.
- *p4 change*, create a new change list.
- *p4 edit -c cl file* and *p4 add -c cl file*.
- *p4 revert -c cl file*.
- *p4 changes -u xxx -s shelved -l*, query change list by user and change list status.
- *p4 shelve -f -c 39231829*, shelve the change list into sever.
- *p4 changes -u yaochuangd -l -c myturing*, query change lists on specified client.
- *p4 reopen -c 39135190 //hw/testgen/mytest.csh*, move opened file into another changelist.

### reference

- [p4 manual](https://www.perforce.com/perforce/r15.2/manuals/cmdref/p4_revert.html)
