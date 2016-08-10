---
layout: post
title: "Speed up performance of iTerm and Vim"
date: 2014-07-16 15:06:30 +0200
category: blog
description: "Improving the performance of iTerm2 and Vim in Mac OS X."
---

I've been a happy user of iTerm2 for a few years, however after moving my entire workflow over to the terminal I experienced a noticeable drop in the responsiveness of my terminal.

Initially I blamed zsh, and particularly [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh), and thus I switched to [fish](http://fishshell.com) and never looked back – My shell was definitely faster, and I even became more productive. However, fish didn't solve my main issue, iTerm2 startup times were slow and Vim jerkiness continued. After researching and trying several things I managed to speed up my terminal and tamed Vim's unresponsiveness, here's how I did it.


### Fast terminal startup time

OS X keeps all kind of system logs under ``/var/log/asl``, after over five years of usage that's a lot of logs, specially if you've gone through several installations of major OS releases and have dealt with all kind of error messages. Cleaning these up definitely helped improving startup times for my terminal, so I'd recommend deleting them.

~~~ bash
sudo rm /var/log/asl/*.asl
~~~

*Don't delete all the files in the ``asl`` directory as it also contains other log files unrelated to the shell.*

Secondly, explicitly invoking the preferred shell instead of relying on what is set as login shell also helped with startup times, for iTerm2 this can be done under Profiles > General > Command, you'll want to input the path for your shell in here, for me that is ``/usr/local/bin/fish -l``.

![Configuration window for iterm@](/assets/images/speed-up-iterm.png)

### Buttery smooth Vim rendering
MacVim's rendering engine is much snappier than Vim's so using mvim instead will greatly improve rendering performance. There is no reason to give up vim in the terminal however, MacVim comes with a CLI mode which can be swapped for the default vim binary via aliases.

~~~ bash
# Install latest MacVim with lua support and cscope
$ brew install macvim --with-cscope --with-lua --HEAD

# $HOME/.config/fish/functions/vim.fish
function vim
  mvim -v $argv
end
~~~

### Living on the edge
Finally, if you are experiencing input lag or any other kind of rendering sluglishness in iTerm you may want to consider running on [nightly builds](http://www.iterm2.com/downloads.html), George Nachman has done an outstanding job improving the performance of the application in the latest releases.
