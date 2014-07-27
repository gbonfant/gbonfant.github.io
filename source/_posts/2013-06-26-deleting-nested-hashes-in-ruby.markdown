---
layout: post
title: "Deleting nested hashes in Ruby"
date: 2013-06-26 20:16:31 +0200
comments: true
description: "How to delete nested hashes in Ruby"
categories: ruby
---
When consuming JSON APIs that return deeply nested JSON data structures you might face a common problem: massaging this data. In this particular case I needed to minimize the payload for a JavaScript application and thus needed to delete unnecessary deeply nested objects.

Although I could've iterated through the original response and deleted anything I didn't need, I've learned to love immutability, and thus I approached the issue by duplicating the response and then deleting the copy's values.

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

This implementation exposes a few flaws. To begin with, both ``dup`` and ``clone`` make shallow copies of the given object, which means that the nested hashes are just references to the original object, because of this, unless the nested hashes in the original object were frozen both methods will mutate the original object state.

Fortunately we can achieve a deep copy of our original object and preserve its integrity using [Marshal](http://ruby-doc.org/core-1.9.3/Marshal.html).

```ruby
class Hash
  def except_nested_key(key)
    copy = Marshal.load(Marshal.dump(self))

    copy.each { |(k, v)| v.delete(key) if v.is_a? Hash }
  end
end
```
