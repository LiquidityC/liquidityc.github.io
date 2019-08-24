---
title:  "Vim quick-compile tips"
layout: post
date:   2019-08-20 13:21
category: vim
tags: vim compile gcc c/c++ code programming
comments: true
---

Ever been hacking on a small program based on something you 
read on the internet, partaking in a hackathon or programming competition
or coding segments based on a book you're reading?

I've been doing that a lot lately and got a bit tired of setting up
different makefiles and the likes to make vim play nice and give me nice
compiler errors.

So I thought, I should add a key command that compiles the file for me...
and one that runs it for me! Huzzah!

This is what I came up with:
```vimscript
au FileType c nnoremap <leader>c :set makeprg=gcc\ -o\ %:t:r\ % <bar> make<cr>
au FileType c nnoremap <leader>r :ter ./%:t:r<cr>

au FileType cpp,cc nnoremap <leader>c :set makeprg=gcc\ -o\ %:t:r\ % <bar> make<cr>
au FileType cpp,cc nnoremap <leader>r :ter ./%:t:r<cr>

au FileType py,python nnoremap <leader>r :ter python %<cr>
```

I'll explain the first two commands and the rest should be rather clear
based on that information.

The first command says the following:
```
For filetype 'c' add a keycommand <leader> followed by 'c' ('\' followed by 'c' on my system) that compiles the current file to a new binary file named after the current file without the file extension. (Eg. test.c -> test)
```

The second command says this:
```
Run a file in a terminal window which is named after the current file without it's file extension.
```

So far this has saved me a ton of hassle when hacking quick c programs based on a book I'm currently
working through.

Hope that it can help you.

// Liq
