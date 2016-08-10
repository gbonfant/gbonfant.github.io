---
layout: post
title: "Embracing (remote) pair programming with tmux and pow!"
date: 2014-08-08 19:24:31 +0200
category: blog
description: "How I use tmux for remote pair programming efficiency and happiness."
---

Allowing your employees to work from home when they need it keeps them motivated and productive.

But, even when working in an office I've found pair programming on separate computers more productive. After all, everyone has their own configuration, bindings, preferred font family, and size, preferred color scheme, their own keyboard layout... So why force ourselves into one screen, one configuration, one "fits it all" environment.

### Enter tmux

I'll spare you the explanation of what tmux is, instead I'll recommend you to go and install it. As much as I read about tmux it wasn't until I started using it that I saw its benefits, and by now every time I'm working I'm attached to a particular session. However, for this particular setup you don't have to be a tmux expert, in fact the simpler you keep it the easier it'll be for your pair to understand and set things up.

### Bootstrapping the environment

Before achieving pair nirvana you'll have to setup an account for your pair to access your machine. If you are pairing with a bunch of people this can easily become unmanageable, that is why I recommend creating an alias to your user account instead.

We can setup an alias on ``System Preferences > Users & Groups``, and then going to **Advanced options** by right clicking on your username. Create an alias and use a distinguishable name, like **pair**.

Instead of implementing a password authentication login we'll use ssh instead. This will make it easier for your pair to connect to your machine - One less password to remember - and it'll give you total control over who connects, as you can simply remove public keys. Open ``/etc/sshd_config`` and change the following settings:

~~~ bash
PasswordAuthentication no
ChallengeResponseAuthentication no
~~~

_You may have to use sudo._

Restart ``sshd`` and get your pair's public key, we'll authorize this key by adding it to ``~/.ssh/authorized_keys`` along with a command to automatically start tmux and attach to our session:

~~~ bash
command="/usr/local/bin/tmux attach -t pair" YOUR_PAIR_PUBLIC_KEY
~~~

Finally create a new tmux session:

~~~ bash
$ tmux new -s pair
~~~

Then have your pair SSH into your machine:

~~~ bash
$ ssh user@hostname
~~~


The best thing out of this setup is that if your pair follows the same procedure, you can have a bidirectional working environment by attaching to each other session. Allowing both of you to pair on the environment with the stroke of a key.

What about debugging running applications while on development? Say something does not look right on of the machines but the bug cannot be seen in the other one.


### Knocking rack apps like a superhero

The guys over at Basecamp have released a [zero-configuration Rack server](http://pow.cx) for OS X that you can set up in seconds, installation is a simple curl command

~~~ bash
$ curl get.pow.cx | sh
~~~

Then symlink the application you are working on

~~~ bash
$ cd ~/.pow

$ ln -s /path/to/your_app
~~~

You can now access your application at ``http://your_app.dev``, which will allow your pair to access it too by appending your local IP and ``.xip.io`` to the application name, like so ``http://your_app.10.0.1.11.xip.io``

Tailing logs and restarting the server can be done just as you would on a remote server - By creating a ``restart.txt`` file and tailing ``development.log``

### Wrap up
Somebody said the first rule of pair programming is making your pair happy, this is easier said than done. Developers are picky, so by allowing my pair and I to work with our preferred settings and in our most familiar environment, achieving that happiness becomes easier.
