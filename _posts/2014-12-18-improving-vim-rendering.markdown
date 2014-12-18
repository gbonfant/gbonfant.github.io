---
layout: post-no-feature
title: "Tips for improving vim rendering"
date: 2014-12-17 13:58:00 +0000
category: blog
description: "Tips for improving vim's rendering"
---

A few months ago I [wrote](http://www.gbonfant.com/blog/speed-up-performance-of-iterm-and-vim/) about maximising Vim performance, particularly along iTerm2. I want to expand on that topic by sharing a few tips that might improve your editor's rendering.

### Cheap gains

`set ttyfast`

Setting this on will send more characters to the screen for redrawing. In general this makes redrawing smoother. Most modern terminals will force this by default anyways, so your mileage may vary.


`set lazyredraw`

If you are using macros constantly (you should) enabling this option will improve redrawing as the screen won't be constantly being redrawn while a macro is being executed.

`set synmaxcol=128`

By far, the most common reason for vim sluggishness is syntax highlighting. Usually this is not a problem if you keep your code to 80 columns and your classes small, however dealing with large JSON or XML files can be quite painfully slow. Setting a lower maximum number of columns in which to search for syntax items will probably prevent vim from being unusable the next time you open that big XML file. Vim's default is 300.

`set nocursorcolumn`

By default this option is set to off but just in case, `cursorcolum` highlights the column the cursor is currently on which naturally makes screen redrawing slower.

`set nocursorline`
Similar to the previous option, this one will highlight the line the cursor is on, once again making screen redrawing slower.


`set norelativenumber`
If you are a fan of relative line numbers you'd be bummed to know that this also slows down redrawing.

### Clean up unused plugins

Perhaps a no-brainer but it's worth mentioning. Getting rid of that plugin you once installed and never used, or learning the native vim implementation of a particular action instead of relying on an external plugin could shave off a few ms worth of performance.


### Share some tips
If you know any tips for improving Vim's performance please share them in the comment section below or reach me on Twitter [@gbonfant](http://twitter.com/gbonfant)
