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

After setting this up you'll want to restart ``sshd`` and get your pair's public key, we'll authorize this key by adding it to ``~/.ssh/authorized_keys`` along with a command to automatically start tmux and attach to our session:

```bash
command="/usr/local/bin/tmux attach -t YOUR_TMUX_SESSION" YOUR_PAIR_KEY
```

Once you are done with the setup everything becomes extremely straightforward: First you create a new tmux session with the same name you passed earlier ``tmux new -s YOUR_TMUX_SESSION`` and then you have your pair SSH into your machine ``ssh user@hostname``


## Taking it further

The cool thing is, if your pair sets up their environment too you can open a new tab on iTerm and ssh into their machine, thus attaching to their tmux session. Now you both can pair while keeping your own environment, bindings and other preferences. This becomes very powerful when combining with tmux/iTerm's split panes and multiple tabs.

Now a problem arises when you have something to debug while keeping the application running. Say one of you make a change to the front-end and the other wants to use a tool such as Chrome Web Inspector to debug it. How can we easily share a running application that is being developed at the same time?


## Knocking rack apps like a superhero

The guys over a Basecamp released a zero-configuration Rack server for OS X that let's you set up a Rack server in seconds[3], installation is as straightforward as it gets:

```bash
$ $ curl get.pow.cx | sh
```

Afterwards you want to symlink the application you are working on

```bash
$ cd ~/.pow
$ ln -s /path/to/myapp
```

Now you can access your app at ``http://myapp.dev`` and your pair can access it by appending ``.xip.io`` and your LAN IP address in the following manner: ``http://myapp.10.0.1.11.xip.io``

Tailing logs and restarting the server can be done just as you would on a real production server. By creating a ``restart.txt`` file and tailing ``development.log``, and just as in a production server you can automate this process with all kind of rake tasks.


## Wrapping it up
Pairing is all about making your pair happy, but developers are picky people, if both me and my pair can work with our preferred settings we both are happy and can focus on the real task at hand.


[^1: Taken from https://gist.github.com/trestrantham/dfc1da1b9580da46001c]
[^2: Although they should really be using a password manager]
[^3: http://pow.cx]
