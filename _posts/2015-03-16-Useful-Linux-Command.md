---
layout: post
title: "Usefull Linux commands" 
date: 2016-03-16
---

This post summaries the commom usefull commands on Linux Platform. 

## Linux Commands

- `pkg-config --static --libs glfw3`  It tells how to link glfw3 dependency libs in correct order  
- `objdump -t a.out` display symbol table of elf file a.out
- `objdump -h a.out` display sections of elf file a.out
- `readelf -p .comment ./a.out` read .comment section info from elf file a.out
- `ls | wc -l` tell how many files in current directory 
- `ls -l | grep ^d` list directory items in current directory
- `lspci | grep -i vga` query graphic cards info as following. Output maybe like this: 
```
	00:02.0 VGA compatible controller: Intel Corporation 3rd Gen Core processor Graphics Controller (rev 09)
```
  In addition, you can query driver info by lspci command, for example: `lspci -vs 00:02.0`. 
  It can tell you which driver module is in use now.   

- `buildcommand 2>&1 | tee build.log` This command can save the build 
   log to a local file and at the same time output it to the screen. A very 
   useful command to analyse build errors.
- `cat /proc/cmdline` Retrieve kernel command line info of current booting 
   from proc file system. When reinstalling a new graphics card, graphic interface
   may not started successfully. So we need to start linux in text mode to install 
   video card driver. In grub edit mode, we can modify linux kernel command line 
   temporarily to let linux start in text mode this time. For example, `BOOT_IMAGE=/vmlinuz-3.10.0-327.4.4.el7.x86_64 root=/dev/mapper/centos-root ro rd.lvm.lv=centos/root rd.lvm.lv=centos/swap crashkernel=auto rhgb quiet rdblacklist=nouveau
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
- `df -lf` query current available volume on mounted devices.
 
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

## VIM tips for quick Reference
----------

- `vim --version | grep clipboard`, It can tell whether vim is able to transfer data with clipboard.
  `sudo apt-get install vim-gnome` in Ubuntu can enhance vim with the abiltiy.
- *Display full file path when editing a file* Sometimes is helpful to us. `:help statusline` and `:help laststatus` give detailed info for reference.
- Sometimes we want *case insensitive* search. `:set ignorecase` or `:set ic` can do that. `:set noic` cancel the previous settings.
- `set ruler` display colum number and row number on the right bottom coner.
- `set incsearch` Vim default behavior is that search begins after you enter pattern. After setting incsearch, incremental search begins.
- `set hlsearch`, highlight matched result.
- `set tabstop=4` Set the number of spaces a *TAB* counts for, default value is 8.
- `set expandtab` When set, use a certain number of space to insert a *TAB*.
- `:set invlist`, Make invisible characters visible. For example, $ represents for enter and ^I for tab. `:set nolist` Makes vim return to normal mode.
- `%` jumps to matching braces. `y%` yanking contents between an item and its matching item. `d%` deleting in the same way. vim object-select can do the similar block 
  selection in virsual mode. `:help object-select` and `:help text-objects`for details.
- vim regular expression [refrence link](http://www.cnblogs.com/PegasusWang/p/3153300.html)
- 3 types of visual modes:
```
	v --> visual mode for multi-character selection and edit    
	V --> visual line mode for multi-line selection and edit    
	Ctrl + v --> visual mode for block selection and edit, it is more flexible    
```
- In visual mode, press *<* and *>* can do auto-indent.

## Grub2
----------

- Edit configure file /etc/default/grub to set kernel command line and other settings.
- On Ubuntu `update-grub` to generate new settings for grub.
- On CentOS `grub2-mkconfig` to generate new settings.
- Parameter *nomodeset*,Adding the nomodeset parameter instructs the kernel to not load video drivers and use BIOS modes instead until X is loaded.

## Shell script
----------

### useful commands
- `echo $?`, query shell exit status. *man bash* and search *special parameters* for more details about Linux bash special parameters.
- `echo $0`, display shell name used by terminal.
- `echo $PATH | awk -F: '{for(i=1; i <= NF; ++i) print "Path: ",$i;}' | grep "bin"`, display PATH content line by line. NF is field number.
   -F: specify ':' as delimeter.
- `awk 'BEGIN{IGNORECASE=1}NR>=1000&&NR<=2000&&(/pattern1/||/pattern2/){print NR,$0}' file`
- *bash* and *csh* has much difference in *if-else* statement
- `grep "patten" file --color=auto`, print matched pattern with highlight.

### regular expression
- `sed -n '/pattern/{=;p}' filename`, print matched lines with its line no.
- `sed -n '/pattern/,+2p' filename`, print matched lines with its following 2 lines.
- `sed -n '/\<aa/{=;p}' filename`,  match word begin with aa while `aa\>` matches word end. 
- `\bxx` also matches word begins with *xx*.
- `sed -n '/^.*R33.*R32.*$/p' file`, print lines which matches both R33 and R32.


### reference
- [awk blog1](http://www.cnblogs.com/ggjucheng/archive/2013/01/13/2858470.html)
- [awk blog2](http://www.cnblogs.com/zhuyp1015/archive/2012/07/14/2591842.html)
- [sed command](http://man.linuxde.net/sed), including it subcommands and regular expression.

## Linux GUI 
----------

### X Windows
- [X Window offical site](https://wiki.archlinux.org/index.php/Xorg)
- `ps aux | grep -i "xinit"` The  `xinit`  program  is  used to start the X Window System server and the first client program
  on systems that are not using a display manager such as xdm or in environments using multiple window systems.  When
  this first client exits, xinit will kill the X server and then terminate.
- `xkill -all` 

## simulate keyboard and mouse event
----------

On windows platform, [autohotky](https://autohotkey.com/) is a good choice and
[xdotool](http://tuxradar.com/content/xdotool-script-your-mouse) for Linux platform.

## miscs
----------

- `control.exe /name Microsoft.ProgramsAndFeatures` Open program windows in command mode.
- Input `chrome://version` into chrome browser to check chrome info.
- [vimvs faq](https://github.com/jaredpar/VsVim/wiki/faq)
- `ffmpeg -ss 03:15:00 -i input-video-name -t 00:48:00 -vcodec copy -acodec copy output-video-name`. This command is used to
  split a piece of video from original video. *-ss xxx* specifies starting time.
  *-t xxx* specifies the length of the video you want to split. *copy* means the same codec with original file.
- `ffmpeg -ss 03:15:00 -i input-video-name -t 00:48:00 -acodec copy -s wxh output-video-name`.  add resize output video size.
- `ffmpeg -ss xx:xx:xx -t xx:xx:xx -i input-video-name -f gif  output-file-name`.
