---
layout: post
title: "Conditional gems in Bundler"
date: 2015-01-27 17:28:00 +0000
description: "Conditional gems in Bundler, or how to survive in a RubyMine team"
category: blog
---

Or how to survive in a RubyMine team.

Conditionally adding gems to a project can be a surprisingly common activity,
specially when the needs of its developers are vastly different
and no consensus is achieved.
And although there's no official way of conditionally adding gems to Bundler
a `Gemfile` is nothing more than a ruby file,
hence ruby code can be executed freely.

The problem with RubyMine is that `require`ing pry or any of its cousins
causes the IDE to blow up.

With these two pieces of information we can update the `Gemfile`
to avoid requiring pry for RubyMine users:


{% highlight ruby %}
if ENV["USE_DEBUGGER"]
  gem "pry-byebug"
  gem "byebug"
  # ...
else
  gem "pry-byebug", require: false
  gem "byebug", require: false
  # ...
end
{% endhighlight %}

This can obviously become kind of cumbersome
if there are many gems that need to be added conditionally
but I haven't found a simpler way to conditionally require gems.

On the topic of Pry,
debugging Ruby applications can be [surprisingly satisfying](http://www.youtube.com/watch?v=4hfMUP5iTq8).
