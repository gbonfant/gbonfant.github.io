---
layout: post
title: "Speed up performance of iTerm and Vim"
date: 2014-07-16 15:06:30 +0200
comments: true
description: "Speed up performance of iTerm2, Terminal.app and Vim in Mac OS X"
categories: vim fish performance
---

Over a year ago I switched to iTerm2 after several years of happily using Terminal.app on Mac OS X, and although I was happy with the change I noticed a drop in the responsiveness of my terminal. Initially I thought zsh, and particularly [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh), were at fault and thus, I switched to [fish](http://fishshell.com). My shell was definitely faster, and after getting used to the non POSIX world of fish I even became more productive. However, it didn't actually solve the issues with the terminal itself, and after moving my Vim environment to the shell I experienced horrible delays in the rendering response and startup of iTerm. Here's how I solved it.

<!-- more -->

## Fast terminal startup time

OS X keeps system logs under ``/var/log/asl``, I've used this laptop for over 5 years and installed every major OS release on top of the previous one, that is perhaps why I had massive amounts of log files, and although you might now be in the same situation it helps to clean them up, so go ahead and run:

```bash
sudo rm /var/log/asl/*.asl
```

> You might not want to delete all the files in the asl directory as it also contains other log files unrelated to the shell.

Explicitly invoking your preferred shell instead of relying on whatever is set as login shell seems to also help with startup times, for iTerm2 this can be done under Profiles > General > Command, you'll want to input the path for your shell in here, for me that is ``/usr/local/bin/fish -l``.

{% img /images/speed_up_iterm.png 'Configuration window for iTerm2' %}

## Buttery smooth Vim rendering
For some reason I haven't been able to figure out yet, MacVim's rendering performance is much better than Vim's so you might want to replace vim with it. Run the following command to install via Homebrew:

```bash
brew install macvim --with-cscope --with-lua --HEAD
```

This will install the latest macvim release with cscope and lua support. If you are like me and like to run Vim in the terminal you don't have to give that up, simply alias your vim command to use macvim in CLI mode. For fish users that can be done by adding the following function to your configuration:

```
function vim
  mvim -v $argv
end
```

## Living on the edge
Ever since making the switch to iTerm I've dealt with the poor refresh rate of text, you might have never seen this problem but for someone with OCD that types relatively fast this has annoyed me to no end and has been the only reason why it took me so long to switch to iTerm and stick with it.

Fortunately after the release of iTerm2 2.0, George Nachman mentioned on HN that he's made significant improvements to the performance of iTerm in the nightly builds, I've been running on nightly ever since I saw his reply and the speed and responsiveness is indeed quite amazing, so go ahead and grab your [nightly build](http://www.iterm2.com/downloads.html).

