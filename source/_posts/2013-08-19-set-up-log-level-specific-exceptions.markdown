---
layout: post
title: "Set-up log level for specific exceptions"
date: 2013-08-19 21:08:49 +0200
comments: true
description: "How to setup log level for a specific exception in Rails"
categories: rails
---

It seems to me that in the Rails world logging is an afterthought, something that can be easily taken care of via 3rd party services, not surprising considering how inexpensive, robust and easy to setup they are.

However, if you are manually keeping track of your logs with something like [graylog](http://graylog2.org) finding any useful, straightforward information about logging in Rails can be daunting. So without any straightforward answer as to how to set a warn level for all those pesky 404 errors that were polluting our logs I decided to peek into Rails' internals. [ActionDispatch::DebugExceptions](http://api.rubyonrails.org/classes/ActionDispatch/DebugExceptions.html) is a simple piece of middleware in charge of logging exceptions, upon further inspection I stumbled upon ``log_error``.

```ruby
def log_error(env, wrapper)
  logger = logger(env)

  return unless logger

  exception = wrapper.exception

  trace = wrapper.application_trace
  trace = wrapper.framework_trace if trace.empty?

  ActiveSupport::Deprecation.silence do
    # string manipulation code ...
    logger.fatal("#{message}\n\n")
  end
end
```

It looks like ``log_error`` is logging every exception as fatal, this is exactly the behaviour I want to modify.

```ruby
class ActionDispatch::DebugExceptions
  def log_error(env, wrapper)
    return if wrapper.exception.is_a? ActionController::RoutingError

    super
  end
end
```

I could as well override the method and add my own logic to it, for instance if I wanted to add a conditional, like so:

```ruby
def log_error(env, wrapper)
  # ...
  if exception.is_a? ActionController::RoutingError
    logger.warn("#{message}\n\n")
  else
    logger.fatal("#{message}\n\n")
 end
```

Middleware code is a joy to read and a great reminder that our code should be simple, lean and understandable.
