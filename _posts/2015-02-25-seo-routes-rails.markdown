---
layout: post
title: "SEO friendly routes in Rails"
date: 2015-02-25 12:20:00 +0000
description: "Build SEO friendly rails urls"
category: blog
---

Considering the URL structure of your site is a very important step towards building applications that are discoverable, and that perform in organic search results. This is particularly important if you are building e-commerce or content applications, where you need consistent, user-friendly, URLs.

By default a restful resource in Rails looks something like this:

{% highlight ruby %}
def show
  @products = Product.find(params[:id])
end
{% endhighlight %}

The problem with this approach is that it generates sitemaps that looks like so:

```
mysite.com/products/1
mysite.com/products/245
mysite.com/products/864
```

Although there are a variety of approaches to solve this problem I'd like to show you an implementation that is easy to maintain and develop. Simply override `to_param`

{% highlight ruby %}
class Product < ActiveRecord::Base
  def to_param
    "#{id}-#{title.parameterize}"
  end
end
{% endhighlight %}

Your sitemap should start looking like below, and no other changes are needed.

```
mysite.com/products/1-red-lego-box
mysite.com/products/245-play-station-4
mysite.com/products/864-beach-ball
```


### How does this work?

[ActiveSupport::Inflector#parameterize](http://api.rubyonrails.org/classes/ActiveSupport/Inflector.html#method-i-parameterize) replaces any special characters with a dash, so that strings like `"Me & Them Jeans"` become `"me-them-jeans"`.

Secondly, Action Pack, which handles view layers, calls [ActiveRecord::Integration#to_param](http://api.rubyonrails.org/classes/ActiveRecord/Integration.html#method-i-to_param) for constructing URLs, this happens behind the scenes any time `link_to` and similar are used in the views.

Finally [ActiveRecord::FinderMethods#find](http://api.rubyonrails.org/classes/ActiveRecord/FinderMethods.html#method-i-find) has an interesting side-effect: if the argument given is a string it'll coerce the value with [String#to_i](http://ruby-doc.org//core-2.2.0/String.html#method-i-to_i), essentially ignoring anything that doesn't look like a number, and returning an integer.
