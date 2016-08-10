---
layout: post
title: "Logging specific exceptions"
date: 2013-08-19 21:08:49 +0200
category: blog
description: "How to manually setup different log levels for specific Rails exceptions."
---

In the Rails world logging seems to be delegated to third party services, these are easy to setup, cheap, and robust.

However, if you are rolling out your own solution with something like [graylog](//graylog2.org), the importance of getting notified of real errors is paramount, otherwise your solution will be just another boy who cried wolf.

In the case of Rails, 404 errors are logged as fatal, meaning that most notifications will be ignored. This won't do. You can either configure graylog or better yet, setup your application so that it doesn't shout when a 404 is rendered.

[ActionDispatch::DebugExceptions](http://api.rubyonrails.org/classes/ActionDispatch/DebugExceptions.html) is a simple piece of middleware in charge of logging exceptions, by taking a look at ``log_error`` it becomes clear what's the behaviour that needs to be changed.


~~~ ruby
def log_error(env, wrapper)
  logger = logger(env)

  return unless logger

  exception = wrapper.exception

  trace = wrapper.application_trace
  trace = wrapper.framework_trace if trace.empty?

  ActiveSupport::Deprecation.silence do
    # ...
    logger.fatal("#{message}\n\n")
  end
end
~~~

The following piece of code will simply ignore any 404 exceptions. That might be enough for simple applications, but in most cases keeping track of how many error pages were rendered will help you spot health issues with your application and further improve the experience of it.

~~~ ruby
class ActionDispatch::DebugExceptions
  def log_error(env, wrapper)
    return if wrapper.exception.is_a? ActionController::RoutingError

    super
  end
end
~~~

Instead, let's add some conditional that logs 404s as warning instead.

~~~ ruby
def log_error(env, wrapper)
  # ...
  if exception.is_a? ActionController::RoutingError
    logger.warn("#{message}\n\n")
  else
    logger.fatal("#{message}\n\n")
 end
~~~
