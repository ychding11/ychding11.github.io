---
layout: post
title: "Usefull Linux commands" 
date: 2016-03-16
---

This post summaries the commonly used commands. 

## Linux Commands

### ELF format

- `objdump -t a.out`, display symbol table of elf file a.out
- `objdump -h a.out`, display sections of elf file a.out
- `readelf -p .comment ./a.out` read .comment section info from elf file a.out

### files & directories
- `ls | wc -l` tell how many files in current directory 
- `ls -l | grep ^d` list directories in current directory

### hardware query
- `lspci | grep -i vga` query graphic cards info.
  Output maybe like this:
  
  ```
  00:02.0 VGA compatible controller: Intel Corporation 3rd Gen Core processor Graphics Controller (rev 09)
  ```
  
  In addition, `lspci` command, for example: `lspci -vs 00:02.0` can tell which driver module is in use now.   
- `sudo fdisk -l`, list storage devices of current system. `sudo fdisk -l /dev/sda` the command can display some detailed info about the device /dev/sda, such as file system type. NOTE: **It requires root**. Use `diskutil list` on Mac OS.
- `df -lf`, query current available volume on mounted devices.
- `lshw -class disk`, lists all available disks in system. `lshw -short -C disk` run this command with root, it can give a summary.
- `sudo lshw -class network`, displays detailed info about network adapter such as discription, product name, vendor, configuration. By these info, we can check device driver version in use. `modinfo driver-name` command can do that. It is helpful to solve issue such as wifi does not work.
- After installed a new graphics card, It may not work. So we need to restart linux in text mode to install graphic card driver.
  In grub edit mode, we can modify linux kernel command line temporarily to let linux start in text mode.
    + `cat /proc/cmdline` Retrieve kernel command line info of current booting from proc file system.
    + `BOOT_IMAGE=/vmlinuz-3.10.0-327.4.4.el7.x86_64 root=/dev/mapper/centos-root ro rd.lvm.lv=centos/root rd.lvm.lv=centos/swap crashkernel=auto rhgb quiet rdblacklist=nouveau 3`
- `sudo apt-get install bcmwl-kernel-source` install BCM43142 wifi driver.
- `mount -t "ntfs" -o ro /dev/sda1 /mountpoint` mount with read-only option.
### package management
- `pkg-config --static --libs glfw3`, It tells how to link glfw3 dependency libs in correct order  
- rpm package manage commands on centos.
    + `rpm -qa`, list all installed package in centos system.
    + `rpm -q --list package-name`, list all files related to package.
    + `rpm -q --state package-name`, query package state. 
### build log
- `buildcommand 2>&1 | tee build.log`  A very useful command to analyze build errors.
- This command can save the build log to a file and at the same time output it to the screen. 
### tr 
It operates on character sets and can be used to translate and delete texts. It can also be used to squeezing repeated characters.
example usage： 

- `tr a-z A-Z`
- `tr '\r\n' ' '`
- `tr -d 't'`
- `echo "This is for testing" | tr -s ' '`
- `echo "my username is 432234" | tr -c 0-9 1`, '-c' means complement.
- `tr 'e' 'E' < fmod_debug_crc1517905402078/gen.log`

### xargs
It builds and executes command lines from standard input. It accepts input from stdin and builds a command with init arguments.

example usage： search keyword like a for loop.

- `cat temp1.txt | xargs -d'\n' grep -o -m 1 "run_tests_grid_composition.py.*[0-9]*" > temp.txt`, 

   - temp1.txt is a file contains a list of file names

   - Command `xargs` builds grep command and executes it for each line of file **temp1.txt** 

reference:

- [xargs' page](https://linux.die.net/man/1/xargs)

## Settings & Configs
- export OptiX_INSTALL_DIR=/home/malei/Downloads/NVIDIA-OptiX-SDK-7.0.0-linux64
- environment settings can be recorded into ~/.profile.
  - .bash_profile and .bashrc are specific to bash,
  - .profile is read by many shells in the absence of their own shell-specific config files. 
  - the idea behind this was that one-time setup was done by .profile and per-shell stuff by .bashrc. 
  - /etc/bash_profile (fallback /etc/profile) is read before the user's .profile for system-wide configuration.
  - Many systems including Ubuntu also use an /etc/profile.d directory containing shell scriptlets.
  - `echo $SHELL`  & `echo $0`  check the the current shell name.
### Configure shell prompt
- $PS1 is the environment variable that store the default prompt setting when we log in the console.
- set $PS1 in .profile so that it can show current git branch
  - `git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'`
  - [configure PS1](https://dev.to/sonyarianto/how-to-add-git-branch-name-to-shell-prompt-in-ubuntu-1gdj)
  - [bash colors](https://www.shellhacks.com/bash-colors/)

```
# function to fetch current git branch name 
git_branch() {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/(\1)/'
}

export PS1="${debian_chroot:+($debian_chroot)}\u@\h:\w \[\033[00;32m\]\$(git_branch)\[\033[00m\]\$"
```

### Package manager
- check package name by dpkg
  - `dpkg -l | grep nvidia-driver | awk '{print $2}' `
  - `dpkg -l | grep vnc | awk '{print $2}' `
- remove a package and it configuration
  - `sudo apt purge tightvncserver`

### Video Driver 
- `lshw -c video` check driver info
  - find following in output `configuration: driver=nvidia`
  - check module info by command `modinfo nvidia`
- `nvidia-smi` is also a good command for NV card
- uninstall NV driver `sudo bash NVIDIA-Linux-x86_64-XXX.XX.run --uninstall`
  - it is a [reference](https://linuxconfig.org/how-to-uninstall-the-nvidia-drivers-on-ubuntu-20-04-focal-fossa-linux) for detailed info
- cuda includes a matching video driver, it is better to use this one, `sudo sh cuda_11.0.2_450.51.05_linux.run`

## Search and Replace

- `locate -b '\qmake'` find qmake location using exact name match. An equal command is `locate -r /qmake$` here *r* does not mean recursive.

- `which nvcc` tell the whole path of nvcc.

### find commonly used options

- `find /home/ding/ -type f`, Find regular files in directory /home/ding. `-type` option specifies file type.  *f* equals to regular file, *d* equals to directory.  

- `find . -type f -size +1000M` , find all files with size bigger than 1000M.

- `find . -type d -empty -delete` , find all empty directories and delete them all.

- `find . -type f -not -name "*.dll"` , find files not matching the patten. [link](http://alvinalexander.com/unix/edu/examples/find.shtml)

- `find ./misc/ -exec file '{}' \;`. Execute `file` command to every file in directory `./misc`. [link](https://shapeshed.com/unix-find/)
    + `-exec` option specifies the command to execute.
    + *;* indicates the ending of command. 
    + `\` is to escape in case of shell interpreting it as another meaning.
    + `{}` stands for the current processing file.   
    
- *find* command works with regular expression. `find . -regex '.*\.\(c\|cpp\|h\)$' -print`, prints whole file name matching the regular expression.
  The command lists all c source files, including cpp files and h files in current directories and its subdirectories.   
    + `-print` append each matching item with new line, it is a default behavior.
    + `-print0` with null character.
  
- *find* has many options, such as *-regex*, *-name*, *-iname*. They are used to tell *find* how to match the *pattern* specified in command line.
    + *-name* only matches file name excluding the pre-directories.  
    + *-iname* the same with *-name*, except that the match is case insensitive.    
    + *-regex* matches whole file path, including pre directories and the regular expression is different with *-name*.

### grep commonly used options

- default match mode is *NOT*  **exactly match whole word** . `'\<question\>'` tells *grep* to match whole world. 
- `grep -i 'question' -r ./`, it searches all files recursively in current directory to match lines containing key word *question*.
- `grep -i question -rl ./`, it lists all files in current directory matching key word *question* instead of displaying matched lines.
- `grep -v pattern filename`, only output "pattern not matched" items.
- `grep -I pattern -r .` ignore binary files.
- `grep -A 3 -B 4 -i 'test' -r .`, *-A 3* print 3 lines before matched lines. 
- `grep -Hn -i "test" -r .`, *-Hn*, print matched lines with its file name and line number.
- `grep -iw "test" -r .`, match the pattern "test" as a whole word.
- `grep -e "-test" -r .`, *-e* , specify a regex, multiple *-e* is *or* in logic.
- `find ../src/ -type f -iname "*.mel" -exec grep -n "menuMode" '{}' \;`, search "menuMode" in all mel files. "{}" represents the current processing file.
- `grep "patten" file --color=auto`, print matched pattern with highlight.
- `grep -n -i "pattern" -r filename | cut -f1 -d:`, print matched line numbers.
- `grep -n -P '\t'  xxx.cpp`, list all lines contains a tab, '\t' is NOT treated as a tab in grep [link](https://askubuntu.com/questions/53071/how-to-grep-for-tabs-without-using-literal-tabs-and-why-does-t-not-work).

## vim configuration
- `vim --version | grep clipboard`, It can tell whether vim is able to transfer data with clipboard.

- `sudo apt-get install vim-gnome`, in Ubuntu it can enhance vim with the abiltiy.

- `:help statusline` and `:help laststatus` give detailed info for reference.*it can show full file path when editing a file* .

- `set ignorecase` or `:set ic`, Sometimes we want *case insensitive* search.

- `set noic` cancel *case insensitive search*. 

- `set ruler` display Colum number and row number on the right bottom corner.

- `set incsearch` Vim default behavior is that search begins after you enter pattern. If setting `incsearch` , **incremental search** begins.

- `set hlsearch`, highlight matched result.

- `set tabstop=4` Set the number of spaces a *TAB* counts for, default value is 8.

- `set expandtab` When set, use a certain number of space to insert a *TAB*.

- `:set invlist`, Make invisible characters visible. For example, $ represents for enter and ^I for tab. `:set nolist` Makes vim return to normal mode.

- `set list  set listchars=tab:>-,trail:$`, visualize tabs. [link](https://vi.stackexchange.com/questions/422/displaying-tabs-as-characters)

- command `%` jumps to matching braces. 

  - `y%` yanking contents between an item and its matching item. 

  - `d%` deleting in the same way. 

  - vim object-select can do the similar block selection in virsual mode. `:help object-select` and `:help text-objects`for details.

- 3 types of visual modes:
```
    v --> visual mode for multi-character selection and edit    
    V --> visual line mode for multi-line selection and edit    
    Ctrl + v --> visual mode for block selection and edit, it is more flexible    
```
- In visual mode, press *<* and *>* can do auto-indent.
- `Shift+V`, select current line and enter visual mode.
- `/\%Vpattern`, search pattern in visual area.
- `:!commandname`, run shell command.
- `:r !commandname`, run shell command and insert the commant output in next line.
- `:%s!\t!    !g`, replace tab with 4 spaces. *!* is delimeter. [link](https://irian.to/blogs/vim-global-command/)

### character range

[123] and [321] define the same character range.
Examples:

| [0-9a-zA-Z] | letters and digits                                                                                              |
|-------------|-----------------------------------------------------------------------------------------------------------------|
| [^A-Z]      | not want to matched character.   "^" will lose its special meaning if it's not the first character in the range |
| "[^"]\+"    | match quoted text                                                                                               |
| \.\s\+[a-z] | match new sentence does not start with a capital letter. \. escape the special meaning of .                     |

### vim regular expression quantifier and metacharacters

These quantifiers are trying to match as much as possible texts.

| Greedy Quantifier | Description                                                                                                        |
|-------------------|--------------------------------------------------------------------------------------------------------------------|
| *                 | matches 0 or more of the preceding characters, ranges or metacharacters .* matches everything including empty line |
| \+                | matches 1 or more of the preceding characters...                                                                   |
| \=                | matches 0 or 1 more of the preceding characters...                                                                 |
| \{n,m}            | matches from n to m of the preceding characters...                                                                 |
| \{n}              | matches exactly n times of the preceding characters...                                                             |
| \{,m}             | matches at most m (from 0 to m) of the preceding characters...                                                     |
| \{n,}             | matches at least n of of the preceding characters...                                                               |
| NOTE              | n > 0 && m > 0                                                                                                     |

Non-greedy quantifiers can be very useful sometimes.

| Non-greedy Quantifier | Description                                                 |
|-----------------------|-------------------------------------------------------------|
| \{-}                  | matches 0 or more of the preceding atom, as few as possible |
| \{-n,m}               | matches 1 or more of the preceding characters...            |
| \{-n,}                | matches at lease or more of the preceding characters...     |
| \{-,m}                | matches 1 or more of the preceding characters...            |
| NOTE                  | where n and m are positive integers (>0)                    |

Metacharacters coming with quantifiers give magic power in regular expression match.

| #  | Matching                                           | #  | Matching                      |
|----|----------------------------------------------------|----|-------------------------------|
| .  | any character except new line                      |    |                               |
| \s | whitespace character                               | \S | non-whitespace character      |
| \d | digit                                              | \D | non-digit                     |
| \x | hex digit                                          | \X | non-hex digit                 |
| \o | octal digit                                        | \O | non-octal digit               |
| \h | head of word character (a,b,c...z,A,B,C...Z and _) | \H | non-head of word character    |
| \p | printable character                                | \P | like \p, but excluding digits |
| \w | word character                                     | \W | non-word character            |

### reference
- [vim regular expression](http://www.cnblogs.com/PegasusWang/p/3153300.html)
- [vim doc](http://vimdoc.sourceforge.net/htmldoc/pattern.html)
- [vimregex](http://vimregex.com/). It is very usefull.

## Grub2
- Edit configure file /etc/default/grub to set kernel command line and other settings.
- On Ubuntu `update-grub` to generate new settings for grub.
- On CentOS `grub2-mkconfig` to generate new settings.
- Parameter *nomodeset*,Adding the nomodeset parameter instructs the kernel to not load video drivers and use BIOS modes instead until X is loaded.

## Shell

### shell in mac os
- `Ctrl + a` Move cursor to the start of line.
- `Ctrl + e` Move cursor to the end of line.
- `echo "set completion-ignore-case On" >> ~/.inputrc` case insensitive auto-complete

### tcsh shell
This shell is a improved c-shell in unix. It is not so friendly to use.
- `setenv VAR XXXX`, set environment variable in terminal.
- `:` is a special character in tcsh terminal.
- *bash* and *csh* has much difference in *if-else* statement

### useful commands
- `echo $?`, query shell exit status. *man bash* and search *special parameters* for more details about Linux bash special parameters.
- `echo $0`, display shell name used by terminal.
- `echo $PATH | awk -F: '{for(i=1; i <= NF; ++i) print "Path: ",$i;}' | grep "bin"`, display PATH content line by line. NF is field number.
   -F: specify ':' as delimeter.
- `awk 'BEGIN{IGNORECASE=1}NR>=1000&&NR<=2000&&(/pattern1/||/pattern2/){print NR,$0}' file`
- `rm -rf `ls amod*.* -d``, remove matched subdirectories in current directory.

### sed command
- `sed -n '/pattern/{=;p}' filename`, print matched lines with its line no.
- `sed -n '/pattern/,+2p' filename`, print matched lines with its following 2 lines.
- `sed -n '/\<aa/{=;p}' filename`,  match word begin with aa while `aa\>` matches word end. 
    - `\bxx` also matches word begins with *xx*.
- `sed -n '/^.*R33.*R32.*$/p' file`, print lines which matches both R33 and R32.
- `sed '/pattern/d -ibak file'`, delete the matched lines and save origin file as backup.
- `sed -n '/tgen.pl.*amodel/p' dl.log | cat | sed 's/^.\{8\}//g'`, How to write multiple expression into one.
- `sed -i 's/Do_Movement/cameraMovement/g' ./shadow_mapping.cpp` Find and replace Do_Movement with cameraMovement in file shadow_mapping.cpp. -i option tells sed to use inplace mode, no temp file created.  
- *sed* and *grep* can cooperate. That is *grep* can suply the file list containing the specified pattern while *sed* edit those files one by one. for example: sed -i 's/pattern/newstr/g' `grep -rl pattern ./`.
- `sed -n '100,200p' filename`, print content from line 100 to line 200.

### reference
- [awk blog1](http://www.cnblogs.com/ggjucheng/archive/2013/01/13/2858470.html)
- [awk blog2](http://www.cnblogs.com/zhuyp1015/archive/2012/07/14/2591842.html)
- [sed command](http://man.linuxde.net/sed), including itsubcommands and regular expression.
- [sed official manual](https://www.gnu.org/software/sed/manual/sed.html), including address mode. 
- [sed chinese doc](http://blog.jobbole.com/109088/#toc_29)
- [sed chinese blog](http://www.cnblogs.com/ctaixw/p/5860221.html), including *regular expression* summary.

## Linux GUI 

### X Windows
- [X Window offical site](https://wiki.archlinux.org/index.php/Xorg)
- `ps aux | grep -i "xinit"` The  `xinit`  program  is  used to start the X Window System server and the first client program
  on systems that are not using a display manager such as xdm or in environments using multiple window systems.  When
  this first client exits, xinit will kill the X server and then terminate.
- `xkill -all` 

## simulate keyboard and mouse event

On windows platform, [autohotky](https://autohotkey.com/) is a good choice and
[xdotool](http://tuxradar.com/content/xdotool-script-your-mouse) for Linux platform.

## miscs

- `control.exe /name Microsoft.ProgramsAndFeatures` Open program windows in command mode.

- Input `chrome://version` into chrome browser to check chrome info.

- [vimvs faq](https://github.com/jaredpar/VsVim/wiki/faq)

### ffmpeg

- `ffmpeg -ss 03:15:00 -i input-video-name -t 00:48:00 -vcodec copy -acodec copy output-video-name`. This command is used to split a piece of video from original video.
  - **-ss xxx** specifies starting time.
  - **-t xxx** specifies the length of the video you want to split. *copy* means the same codec with original file.

- `ffmpeg -ss 03:15:00 -i input-video-name -t 00:48:00 -acodec copy -s wxh output-video-name`.  add resize output video size.

- `ffmpeg -ss xx:xx:xx -t xx:xx:xx -i input-video-name -f gif  output-file-name`.
  - extract video to gif image

- `ffmpeg.exe -r 5 -start_number 0 -i %04d.png -c:v hevc_nvenc -pix_fmt yuv420p out.mp4`
  - encode image sequence into video
