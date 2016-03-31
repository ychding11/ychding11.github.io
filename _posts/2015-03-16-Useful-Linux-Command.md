---
layout: post
title: "Usefull Linux Commands" 
date: 2015-03-16
---
- `pkg-config --static --libs glfw3`  tell you how to link glfw3 lib in correct order  
- `objdump -t a.out` display symbol table of elf file a.out
- `objdump -h a.out` display sections of elf file a.out
- `readelf -p .comment ./a.out` read .comment section info from elf file a.out
- `ls | wc -l` tell how many files in current directory 
- `ls -l | grep ^d` list directory items in current directory
- `lspci | grep -i vga` query graphic cards info   
```
00:02.0 VGA compatible controller: Intel Corporation 3rd Gen Core processor Graphics Controller (rev 09)
```   
You can query driver infomation by lspci command as `lspci -vs 00:02.0`. It can tell you what driver is in use now.   
- `buildcommand 2>&1 | tee build.log` this command can save the build log to a local file at the same time output it 
 to the screen. A very useful command to analyse build errors.
- `cat /proc/cmdline` Retrieve kernel command line info for current booting from proc file system.
 
## Search and Replace

- `locate -b '\qmake'` find qmake path using exact name. Another equal command is `locate -r /qmake$` here *r* 
does not mean recursive.
- `find /home/ding/ -type f` Find regular files in directory /home/ding. -type option specifies file type. 
f equals to regular file, d equals to directory.  
- `find ./misc/ -exec file '{}' \;` Execute file command to every file in directory ./misc. -exec option 
specifies the command to execute, ';' indicates the ending of command. \ is to escape in case of shell 
interpret it as another meaning. {} stands for the current processing file.   
- *find* command matchs file name other than file content. `find . -regex '.*\.\(c\|cpp\|h\)$' -print` print whole file 
name matching the regular expression. `-print` append each matching item with new line, default behavior, while `-print0`  
with null character. It lists all c files, cpp files and h files in current directories and its subdirectories.   
- *find* has many options, such as *-regex*, *-name*, *-iname*. They belog to the same category telling *find* 
how to match the specified *pattern* in command line. Command manual has detail info, here I give a brief description.   
*-name* only matches file name excluding the pre directories.  
*-iname* the same with *-name*, except that the match is case insensitive.    
*-regex* matches whole file path, including pre directories and regular expression is different with *-name*.
- `grep -i 'question' -r ./` Search all files recursively in current directory to find lines containing key word *question*.  
Another grep command `grep -i question -rl ./`, it lists all files in current directory containing the key 
word *question* instead of displaying matched lines. Default match mode is *NOT* exact whole world match.    
`'\bquestion\b'` pattern tell *grep* matches whole world only. 
- `sed -i 's/Do_Movement/cameraMovement/g' ./shadow_mapping.cpp` Find and replace Do_Movement with cameraMovement 
in file shadow_mapping.cpp. -i option tells sed to use inplace mode, no temp file created.  
- *sed* and *grep* can cooperate. That is *grep* can suply the file list containing the specified pattern while 
*sed* edit those files one by one. for example: sed -i 's/pattern/newstr/g' `grep -rl pattern ./`.

## VIM Tips for quick Reference

- *Display full file path when editing a file* Sometimes It is helpful to us. `:set statusline+=%F` serves that purpose.     
`:set laststatus=2` to make the status bar visible. `:help statusline` and `:help laststatus` can give detailed info. 
- Sometimes we want *case insensitive* search. `:set ignorecase` or `:set ic` can do that. `:set noic` reset the previous
settings.
