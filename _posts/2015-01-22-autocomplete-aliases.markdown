---
layout: post
title: "Alias autocompletion for fish"
date: 2015-01-22 10:50:00 +0000
description: "Auto-completion for aliases in fish shell"
category: blog
---

Back in August, proper autocompletion for aliases [was added](https://github.com/fish-shell/fish-shell/commit/06400b83b188156f31ed92216ec27122d29f88cd) to fish.
At the moment of this writing there's no real UI for it yet,
however you can already start using it by keeping up with the master branch.

~~~ bash
$ brew update; and brew upgrade --HEAD fish
~~~

You can now use the ``--wraps`` flag to
recursively inherit the completions of a wrapped command.

For instance:

~~~ bash
function g --wraps git
  git $argv
end
~~~

Now doing something like ``g co <tab>`` will autocomplete the branch names:
<img src="/assets/images/fish-autocomplete.png" alt="fish alias autocompletion" with=250 />
