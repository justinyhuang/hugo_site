---
title: Keep Your Fingers Away from "ESC"
date: 2011-12-11T02:05:00
---

When editing in Vim the keys that I hate most are "ESC" (Esc, Shift and CapsLock). They interrupt my thoughts when I have to find the keys far, far away from the comfort area of my fingers, not to mention the mistypes like pressing tab when I mean CapsLock.

<!--more-->

There are many ways to get rid of these keys, below is my recipe.
No guarantee you can throw the "ESC" keys away, but I would say your fingers will stay away from "ESC" for 80% of your typing experience.

In your vim configuration file, .vimrc if you are in Linux, set the mappings as:


{{< highlight vim >}}
    "map space to @ (shift-2) for easier use the record buffer
    map <space> @

    "jumping among windows, with the arrow keys
    nnoremap <silent> <Down> <C-W>j
    nnoremap <silent> <Up> <C-W>k
    nnoremap <silent> <Left> <C-W>h
    nnoremap <silent> <Right> <C-W>l

    " not to even think about the <ESC> key
    imap jj <ESC>

    "delete a word in insert mode
    imap jjd <ESC>diwi

    "capitalize the first character of the current word, in Insert mode
    imap jjc <ESC>b~A

    " Swap ; and :, save the Shift key.
    nnoremap ; :
    nnoremap : ;

    "page down
    map ,j <C-d>
    "page up
    map ,k <C-u>
{{< /highlight >}}
