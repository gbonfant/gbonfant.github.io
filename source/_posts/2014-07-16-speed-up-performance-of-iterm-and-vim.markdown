---
layout: post
title: "Speed up performance of iTerm and Vim"
date: 2014-07-16 15:06:30 +0200
comments: true
description: "Speed up performance of iTerm2, Terminal.app and Vim in Mac OS X"
categories: vim fish performance
---

Over a year ago I switched to iTerm2 after several years of happily using Terminal.app on Mac OS X, and although I'm more than happy with the change I've noticed that in the last weeks the overall responsiveness of my terminal has declined. Initially I thought zsh and particularly [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) were at fault, and thus I switched to [fish](http://fishshell.com), my shell was definitely faster, and after getting used to the non POSIX world of fish I even became more productive. However, it didn't actually solve the issues with the terminal itself, and after moving my Vim environment to the shell I experienced horrible delays in the rendering response and startup of iTerm.

## How I solved it.

First of all I tackled the slow starting time on iTerm by deleting the system logs OS X keeps in ``/var/log/asl``. After some years of usage without any clean os install they just piled up.

```bash
sudo rm /var/log/asl/*.asl
```

> You might not want to delete all the files in the asl directory as it also contains other log files unrelated to the shell.

<!-- more -->

Secondly, I changed the startup shell from its default to ``/usr/local/bin/fish -l``, this can be done under Profiles > General > Command in iTerm 2 and the value you need to add will depend on which shell you are using and where in your filesystem it is located, for instance you might want to set it up to ``/bin/bash -l`` if you are using bash.

{% img /images/speed_up_iterm.png 'Configuration window for iTerm2' %}

This improved the overall snappiness of iTerm2 but I still had an issue with the rendering performance of Vim.

## Making Vim buttery smooth
This was an easy one, MacVim is fast, why Vim isn't? I still haven't found an answer to this question but I had no intention of going back to running Vim on a separate GUI shell, fortunately you can use MacVim as a CLI by appending the flag ``-v`` then simply alias it to vim, for fish that would be:

```
function vim
  mvim -v $argv
end
```

Now your iTerm should be faster and Vim rendering smoother.
