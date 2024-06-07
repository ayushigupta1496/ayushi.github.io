---
layout: post
title: 06-06-2024
---

#                                VIM EDITOR
- Vim, short for Vi IMproved, is a highly configurable text editor built to enable efficient text editing. It’s an enhanced version of the Vi editor, which was developed back in the 1970s. Vim is widely used by programmers, system administrators, and anyone who spends a significant amount of time working in the terminal

# MODES OF VIM
- One of the unique features of Vim is its modes. Vim has several modes, each serving a different purpose:

1. Normal Mode: This is the default mode when you first open Vim. In this mode, you can navigate the file, delete text, copy text, and perform other commands.You change modes by pressing <ESC> (the escape key) to switch from any mode back to Normal mode
2. Insert Mode: This mode allows you to insert and edit text. To enter Insert Mode from Normal Mode, press `i`.
3. Visual Mode: In this mode, you can visually select blocks of text. To enter Visual Mode from Normal Mode, press `v`.
4. Command-Line Mode: This mode lets you enter Vim commands. To enter Command-Line Mode from Normal Mode, press ':'

# Command-line
- Command mode can be entered by typing : in Normal mode. Your cursor will jump to the command line at the bottom of the screen upon pressing :. This mode has many functionalities, including opening, saving, and closing files, and quitting Vim.

1. :q! quit (close window)
2. :w! save (“write”)
3. :wq! save and quit
4. :ls show open buffers
5. :help {topic} open help

# Navigating in Vim:
- Moving the Cursor: Use the arrow keys or `h`, `j`, `k`, `l` keys to move left, down, up, and right respectively.
- Jumping to the Beginning or End of a Line: Press `0` to jump to the beginning of a line and $ to jump to the end.
- Jumping to a Specific Line: Type `:<line_number>` and press Enter to jump to a specific line.
- File: gg (beginning of file), G (end of file)


**Basic Commands in Vim**
- Always use the Esc key to go into normal mode and use the insertion, deletion keys, and other keys. 

1. dw- To delete the word move the cursor to the beginning of the word and use dw command in normal mode. The word under the cursor will be deleted.
2. d2w- To delete more than one word in a single line use the following command.
3. d$- To delete the line move cursor to the beginning of the line and use d$ command in normal mode. The line under the cursor will be deleted.
4. u - for undo
5. ctrl+r -for redo
6. :/word - to search a word
7. y - to copy
8. p - to paste
9. d/x - to delete
10. ctrl+v - to edit multiple lines
11. dd - to delete multiple lines
12. :g/[pattern]/d – To delete the lines containing the pattern
13. :%d – Deletes all the lines from the file
14. :.,$d – Deletes the lines from current line to the end of file
15. :1,.d – Deletes the lines from the starting of file to the current line
16. :set number- to set numbers on lines in  any vim file


 **wc command** -It is used to find out number of lines, word count, byte and characters count in the files specified in the file arguments.
- wc [OPTION]... [FILE]...


## Confriguring Vimrc



