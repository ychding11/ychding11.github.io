---
layout: post
title: "Usefull Linux Commands" 
date: 2015-03-16
---

This post summaries the commom usefull commands on Linux Platform. 

## Linux Commands

- `pkg-config --static --libs glfw3`  It tells how to link glfw3 dependency libs in correct order  
- `objdump -t a.out` display symbol table of elf file a.out
- `objdump -h a.out` display sections of elf file a.out
- `readelf -p .comment ./a.out` read .comment section info from elf file a.out
- `ls | wc -l` tell how many files in current directory 
- `ls -l | grep ^d` list directory items in current directory
- `lspci | grep -i vga` query graphic cards info as following. ` 00:02.0 VGA compatible controller: Intel Corporation 3rd Gen Core processor Graphics Controller (rev 09)`   
   You can query driver infomation by lspci command: `lspci -vs 00:02.0`.  It can tell you what driver is in use now.   
- `buildcommand 2>&1 | tee build.log` This command can save the build 
   log to a local file and at the same time output it to the screen. A very 
   useful command to analyse build errors.
- `cat /proc/cmdline` Retrieve kernel command line info of current booting 
   from proc file system. When reinstalling a new graphics card, graphic interface
   may not started successfully. So we need to start linux in text mode to install 
   video card driver. In grub edit mode, we can modify linux kernel command line 
   temporarily to let linux start in text mode this time. For example,
   `BOOT_IMAGE=/vmlinuz-3.10.0-327.4.4.el7.x86_64 root=/dev/mapper/centos-root ro rd.lvm.lv=centos/root rd.lvm.lv=centos/swap crashkernel=auto rhgb quiet rdblacklist=nouveau
    3 `
- `sudo fdisk -l` List storage devices of current system. It is usefull when 
   mounting a movable device but don't know it's device name. `sudo fdisk -l /dev/sda` 
   for example, the command can display some detailed info about the device /dev/sda,
   such as file system type. NOTE: this command needs root.
- `lshw -class disk` command lists all available disk in system.
  `lshw -short -C disk` run this command with root, it can give a summary.
- `sudo lshw -class network` command displays the detailed info 
  about network adapter such as discription, product name, vendor,
  configuration and so on. By these info, we can check device driver
  version in use. `modinfo driver-name` command can do that. It is
  very helpful to solve problems such as wifi does not work.
- ` sudo apt-get install bcmwl-kernel-source` install BCM43142 wifi driver.
- `mount -t "ntfs" -o ro /dev/sda1 /mountpoint` mount with read-only option.
- `rpm -qa`, list all installed package in centos system. `rpm -q --list package-name` 
  list all files related to package. `rpm -q --state package-name`, query package state. 
 
## Search and Replace

- `locate -b '\qmake'` find qmake location using exact name match.
  Another equal command is `locate -r /qmake$` here *r* does not mean recursive.
- `find /home/ding/ -type f` Find regular files in directory /home/ding.
  `-type` option specifies file type.  f equals to regular file, d equals to directory.  
- `find ./misc/ -exec file '{}' \;`. Execute `file` command to every file in directory
  `./misc`. `-exec` option specifies the command to execute. ';' indicates the ending
  of command. `\` is to escape in case of shell interpreting it as another meaning.
  `{}` stands for the current processing file.   
- *find* command matchs file name other than file content. 
  `find . -regex '.*\.\(c\|cpp\|h\)$' -print` print whole file name matching 
  the regular expression. `-print` append each matching item with new line, 
  it is a default behavior, while `-print0` with null character. The command 
  lists all c source files, including cpp files and h files in current 
  directories and its subdirectories.   
- *find* has many options, such as *-regex*, *-name*, *-iname*. They belog 
  to the same category, telling *find* how to match the specified *pattern* 
  in command line. Command manual has detail info, here I just give a brief 
  description.  *-name* only matches file name excluding the pre-directories.  
  *-iname* the same with *-name*, except that the match is case insensitive.    
  *-regex* matches whole file path, including pre directories and regular 
  expression is different with *-name*.
- `grep -i 'question' -r ./` Search all files recursively in current directory 
  to find lines containing key word *question*. Another grep command 
  `grep -i question -rl ./`, it lists all files in current directory containing key 
   word *question* instead of displaying matched lines. Default match mode is *NOT* 
   exact match the whole word. `'\<question\>'` pattern tell *grep* matches whole 
   world only. 
- `find ../src/ -type f -iname "*.mel" -exec grep -n "menuMode" '{}' \;` search 
  "menuMode" in all mel files. "{}" represents the current processing file.
- `sed -i 's/Do_Movement/cameraMovement/g' ./shadow_mapping.cpp` Find and replace Do_Movement with cameraMovement 
   in file shadow_mapping.cpp. -i option tells sed to use inplace mode, no temp file created.  
- *sed* and *grep* can cooperate. That is *grep* can suply the file list containing the specified pattern while 
  *sed* edit those files one by one. for example: sed -i 's/pattern/newstr/g' `grep -rl pattern ./`.

## VIM Tips for quick Reference

- `vim --version | grep clipboard` It can tell whether vim is able 
  to transfer data with clipboard. `sudo apt-get install vim-gnome` 
  in ubuntu can enhance vim with the abiltiy.
- *Display full file path when editing a file* Sometimes is helpful to us.
  bar visible. `:help statusline` and `:help laststatus` give detailed info for 
  reference.
- Sometimes we want *case insensitive* search. `:set ignorecase` or `:set ic` 
  can do that. `:set noic` reset the previous settings.
- `set ruler` display colum number and row number on the right bottom coner.
- `set incsearch` Vim default behavior is that search begins after you enter pattern.
  When set incsearch, incremental search begins.
- `set tabstop=4` Set the number of spaces a *TAB* counts for, default value is 8.
- `set expandtab` When set, use a certain number of space to insert a *TAB*.
- `%` jumps to matching braces. `y%` yanking contents between an item and its matching 
  item. `d%` deleting in the same way. vim object-select can do the similar block 
  selection in virsual mode. `:help object-select` and `:help text-objects`for details.
- `:set invlist` Makes invisible characters visible. For example, $ for enter and ^I 
  for tab. `:set nolist` Makes vim return to normal mode.
- vim regular expression[link](http://www.cnblogs.com/PegasusWang/p/3153300.html)
+ Input `chrome://version` into chrome browser to check chrome info.

## Grub2

- Edit configure file /etc/default/grub to set kernel command line and other settings.
- On Ubuntu `update-grub` to generate new settings for grub.
- On CentOS `grub2-mkconfig` to generate new settings.
- Parameter *nomodeset*,Adding the nomodeset parameter instructs the kernel to not load video drivers and use BIOS modes instead until X is loaded.

## Shell script

- *echo $?* query shell exit status. *man bash* and search *special parameters* for more details.

## X Windows

- [X.org](https://wiki.archlinux.org/index.php/Xorg)
- *ps aux | grep -i "xinit"* The  xinit  program  is  used to start the X Window System server and a first client program
  on systems that are not using a display manager such as xdm or in environments that use multiple window systems.  When
  this first client exits, xinit will kill the X server and then terminate.
- *xkill -all* 

## Others

- `control.exe /name Microsoft.ProgramsAndFeatures`
- [vimvs faq](https://github.com/jaredpar/VsVim/wiki/faq)
- `ffmpeg -ss 03:15:00 -i input-video-name -t 00:48:00 -vcodec copy -acodec copy output-video-name`. This command is used to split a piece of video from original video. *-ss xxx* specifies starting time. *-t xxx* specifies the length of the video you want to split. *copy* means the same codec with original file.
