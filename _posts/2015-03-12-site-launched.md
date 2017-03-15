---
layout: post
title: "Launches Site"
date: 2015-03-11
---

Today, I launches my tech blog site on github. This post gives a summary about 
how to setup jekyll environment and git operation.

## configure external git difftool

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
