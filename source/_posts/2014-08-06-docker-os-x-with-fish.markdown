---
layout: post
title: "Docker in OS X with fish"
date: 2014-08-06 21:01:29 +0200
comments: true
description: "Installing Docker in OS X with fish shell"
categories: unix
---

Docker is intended to be ran in Linux environments, but thanks to [Boot2Docker](https://github.com/boot2docker/boot2docker), a lightweight Linux distribution specially made for this purpose, we can run docker in a mac environment with ease.

Before starting out make sure you are running VirtualBox, get it either via a direct [download link](https://www.virtualbox.org/wiki/Downloads) or via [homebrew-cask](http://caskroom.io/). Better yet you could set it up with something like [Vagrant](http://www.vagrantup.com/), the choice is up to you.

Once you have VirtualBox the next steps are as straightforward, first get boot2docker via homebrew ``$ brew install boot2docker ``.

This will not only install the aforementioned Linux distro but also docker itself. Before continuing you want to make sure you set the ``DOCKER_HOST`` environment variable needed for the docker client to connect to the VM.

```bash
# Set the following in $HOME/.config/fish/config.fish
set -x DOCKER_HOST "tcp://192.168.59.103:2375"
```

Afterwards you'll want to run ``$ boot2docker init ``, this will download an ISO image and mount it as our docker VM.

Finally simply start the VM with ``$ boot2docker up ``.

And that's it! If you want, make sure everything is running smoothly by running docker's example image `` docker run hello-world ``.
