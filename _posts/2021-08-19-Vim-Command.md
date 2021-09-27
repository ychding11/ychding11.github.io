---
layout: post
title: "Vim command" 
date: 2021-08-19
---

This post summaries the commonly used vim commands and examples. 

## vim 

### vim configuration
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

### vim command 
- command `%` jumps to matching braces. 

  - `y%` yanking contents between an item and its matching item. 

  - `d%` deleting in the same way. 

  - vim object-select can do the similar block selection in visual mode. `:help object-select` and `:help text-objects`for details.

- 3 types of visual modes:

  ```bash
    v --> visual mode for multi-character selection and edit    
    V --> visual line mode for multi-line selection and edit    
    Ctrl + v --> visual mode for block selection and edit, it is more flexible    
  ```
- In visual mode, press *<* and *>* can do auto-indent.

- `Shift+V`, select current line and enter visual mode.

- `/\%Vpattern`, search pattern in visual area.

- `:!command`, run shell command.

- `:r !command`, run shell command and insert the command output in next line.

- `:%s!\t!    !g`, replace tab with 4 spaces. *!* is delimiter, see [details](https://irian.to/blogs/vim-global-command/)

### character range


| pattern     | letters and digits                                                                                              |
| ------------|---------------------------------------------------------------------------------------------------------------- |
| [^A-Z]      | not want to matched character.   "^" will lose its special meaning if it's not the first character in the range |
| "[^"]\+"    | match quoted text                                                                                               |
| \.\s\+[a-z] | match new sentence does not start with a capital letter. \. escape the special meaning of .                     |

- Note: [123] and [321] define the same character range.

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
- [vimregex](http://vimregex.com/). It is very useful.

