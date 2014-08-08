---
layout: post
title: "Embracing (remote) pair programming with tmux and pow!"
date: 2014-08-08 19:24:31 +0200
comments: true
categories: productivity
---

Many articles and books have been written about the benefits of pair programming, on that same line many companies are realising that is easier to hire from a pool of worldwide talent rather than a regional pool, or that allowing your workers to work from home when they need it keeps people motivated and productive.

But, even if you don't work remotely I've found that pairing on separate computers makes me and my pair happier and more productive. After all, everyone has their own configuration, bindings, preferred font family, and size, preferred color scheme, their own keyboard layout, and the list goes on and on... So why force ourselves into one screen while straining our necks and eyes.

## Enter tmux

Much has been written about tmux, so I'll spare you the explanation and just recommend you to go and install it. As much as I read about tmux it wasn't until I started using it that I saw its benefits. For my particular setup I don't even use tmux to its maximum potential, and that is okay, I want to keep my pairing setup as lightweight as possible as I've found that the more complicated your setup is, the less likely your pair will want to try it out.

## Setting up the environment

Knowing the username of every developer you end up pairing quickly becomes cumbersome, so we'll create an alias to our main user account. This can easily be done in OS X under ``System Preferences > Users & Groups``, go to _Advanced options_ by right clicking on your username and create an alias for it.

The same thing can be accomplished with the following command: ``sudo dscl . -append /Users/$USER RecordName Pair pair``[1]

Password authentication has given me headaches more often than not, and most likely your pair will thank you for having to remember one last password[2], so the next steps involve turning password authentication off and allowing public key authentication instead. This can be done in ``/etc/sshd_config`` by setting the following options:

```bash
PasswordAuthentication no
ChallengeResponseAuthentication no
```

After setting this up you'll want to restart ``sshd`` and get your pair's public key, we'll authorize this key by adding it to ``~/.ssh/authorized_keys``


[^1: Taken from https://gist.github.com/trestrantham/dfc1da1b9580da46001c]
[^2: Although they should really be using a password manager]
