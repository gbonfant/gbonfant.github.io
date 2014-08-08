---
layout: post
title: "Embracing (remote) pair programming with tmux and pow!"
date: 2014-08-08 19:24:31 +0200
comments: true
categories: productivity
---

Many articles and books have been written about the benefits of pair programming, on that same line many companies are realising that is easier to hire from a worldwide pool of talent rather than a, sometimes limited, regional pool. If not allowing your employees to work from home when they need it keeps them motivated and productive.

But, even if you don't work remotely I've found pair programming on separate computers makes my pair and I happier and more productive. After all, everyone has their own configuration, bindings, preferred font family, and size, preferred color scheme, their own keyboard layout... So why force ourselves into one screen, one configuration, one _fits it all_ environment while straining our necks and eyes.

<!-- more -->

## Enter tmux

Much has been written about tmux, so I'll spare you the explanation and just recommend you to go and install it. As much as I read about tmux it wasn't until I started using it that I saw its benefits. For this particular setup you don't have to be a tmux expert, I don't use tmux to its maximum potential, and that is okay, I want to keep my pairing setup as lightweight as possible as I've found that the more complicated your setup is, the less likely your pair will want to try it out.

## Bootstrapping the environment

Before achieving pair nirvana you'll have to setup a few things. Knowing the username of every developer you end up pairing quickly becomes cumbersome, so we'll create an alias to our main user account. This can easily be done in OS X under ``System Preferences > Users & Groups``, and then going to _Advanced options_ by right clicking on your username. Once there create an alias for your username, this will be used by your current and any future pair.

The same thing can be accomplished with the following command ``sudo dscl . -append /Users/$USER RecordName Pair pair``[^1]

Since password authentication has given me headaches more often than not, and most likely your pair will thank you for having to remember one least password, the next step involves turning password authentication off and allowing public key authentication instead for ssh connections. This can be done in ``/etc/sshd_config`` by setting the following:

```bash
PasswordAuthentication no
ChallengeResponseAuthentication no
```

By now you'll want to restart ``sshd`` and get your pair's public key, we'll authorize this key by adding it to ``~/.ssh/authorized_keys`` along with a command to automatically start tmux and attach to our session:

```bash
command="/usr/local/bin/tmux attach -t YOUR_TMUX_SESSION" YOUR_PAIR_KEY
```

From now on things becomes extremely straightforward, first you create a new tmux session ``tmux new -s YOUR_TMUX_SESSION`` and then you have your pair SSH into your machine ``ssh user@hostname``.


## Taking it further

The cool thing is, if your pair sets up their environment too, you can open a new tab on iTerm and ssh into their machine, thus attaching to their tmux session. Now you both can pair while keeping your own environment, bindings and other preferences. This becomes very powerful when combining with tmux/iTerm's split panes and multiple tabs.

The remaining issue to tackle is debugging running applications, say one of you makes a change to the front-end, tests are passing but something still doesn't look right, at this point the pair not coding wants to fire the browser and use a tool such as Chrome Web Inspector to debug it. How can we easily share a running application that is being developed while keeping our sanity and avoiding screen sharing software?


## Knocking rack apps like a superhero

The guys over at Basecamp released a zero-configuration Rack server for OS X that you can set up in seconds[^2], installation is as straightforward as it gets:

```bash
$ $ curl get.pow.cx | sh
```

Afterwards you want to symlink the application you are working on

```bash
$ cd ~/.pow
$ ln -s /path/to/your_app
```

You can now access this application at ``http://your_app.dev``, furthermore your pair can access it too by appending your local IP and ``.xip.io`` to the application name like so ``http://your_app.10.0.1.11.xip.io``

Tailing logs and restarting the server can be done just as you would on a remote server. That is, by creating a ``restart.txt`` file and tailing ``development.log``, just as with a remote server you can automate this process with all kind of rake tasks.


## Wrapping it up
Pair programming is all about making your pair happy, but developers are picky people, if both my pair and I can work with our preferred settings we both are happy and can focus on the real task at hand.

[^1]: Taken from [here](https://gist.github.com/trestrantham/dfc1da1b9580da46001c)
[^2]: http://pow.cx
