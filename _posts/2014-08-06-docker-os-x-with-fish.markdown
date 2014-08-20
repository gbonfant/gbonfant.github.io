---
layout: post-no-feature
title: "Docker in OS X with fish"
date: 2014-08-06 21:01:29 +0200
category: blog
description: "Making Docker play nicely with fish."
---

Docker is intended to be ran in Linux environments, but thanks to [Boot2Docker](https://github.com/boot2docker/boot2docker), a lightweight Linux distribution specially made for this purpose, we can run it in a mac environment with ease.

*Before starting out make sure you have [VirtualBox](https://www.virtualbox.org/wiki/Downloads) installed.*

### Installation
By using homebrew, boot2docker installation is as easy as it gets.

{% highlight bash %}
$ brew install boot2docker
{% endhighlight %}

With a single command we get both boot2docker and Docker itself.

### Configuration
On POSIX shells like bash or zsh boot2docker should work out of the box. With fish however, there is an extra step that needs to be done, that is setting the ``DOCKER_HOST`` environment variable needed for our docker client to connect to the virtual machine.

{% highlight bash %}
# $HOME/.config/fish/config.fish
set -x DOCKER_HOST "tcp://192.168.59.103:2375"
{% endhighlight %}

Afterwards, we need to download the Linux VM and mount it on VirtualBox, boot2docker takes care of this step with the following command:

{% highlight bash %}
$ boot2docker init
{% endhighlight %}

Then simply start the VM with:

{% highlight bash %}
$ boot2docker up
{% endhighlight %}


Finally, make sure everything is properly setup by running docker's example image:

{% highlight bash %}
$ docker run hello-world
{% endhighlight %}
