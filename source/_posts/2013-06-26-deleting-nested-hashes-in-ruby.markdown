---
layout: post
title: "Deleting nested hashes in Ruby"
date: 2013-06-26 20:16:31 +0200
comments: true
description: "How to delete nested hashes in Ruby"
categories: ruby
---

I wanted to expose a method for performing a relatively common problem I was facing: removing nested items in a data structure. I tried to aproach the problem by naively duplicating the object and deleting any nested hash.

```ruby
class Hash
  def except_nested_key(key)
    dup.except_nested_key!(key)
  end

  def except_nested_key!(key)
    each { (k, v) v.delete(key) if v.is_a? Hash }

    self
  end
end
```

This implementation exposes a few flaws. To begin with, both ``dup`` and ``clone`` make shallow copies of the given object, which means our nested hashes are just references to the original object, because of this, unless the nested hashes in the original object were frozen both methods will mutate the original object state! [^1]

Fortunately we can achieve a deep copy of our original object and preserve its integrity using [Marshal](http://ruby-doc.org/core-1.9.3/Marshal.html).

```ruby
class Hash
  def except_nested_key(key)
    copy = Marshal.load(Marshal.dump(self))

    copy.each { |(k, v)| v.delete(key) if v.is_a? Hash }
  end
end
```

[^1]: _Object-oriented programming makes code understandable by encapsulating moving parts. Functional programming makes code understandable by minimizing moving parts._ — Michael Feathers
