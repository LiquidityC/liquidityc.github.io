---
title:  "Debugging rust in vim"
layout: post
author: Linus Probert
date:   2020-04-25 14:10
tags:
categories:
comments: true
---

So, another issue, another solution. I have been a great big fan of the vim
functionality "termdebug" ever since it was introduced with vim 8.1. You can
read more about it
[here](https://www.dannyadam.com/blog/2019/05/debugging-in-vim/) and in the
help files under ":help terminal-debug".

As none of you might have noticed I'm pretty big on [rust](https://www.rust-lang.org). As it happens they ship with a reall nice wrapper for gdb, namely 'rust-gdb'. Naturally I wanted that wrapper to work through my default debugging tool within vim.

Here's how it's done:
```vimscript
packadd termdebug "Enable terminal-debug
let termdebugger = "rust-gdb" "Use rust-gdb instead of default gdb
```

That's it, now you get nice printing in gdb for rust within vim.

Enjoy!

// Liq
