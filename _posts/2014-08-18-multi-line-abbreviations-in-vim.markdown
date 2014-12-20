---
layout: post
title: "Multi-line abbreviations in Vim"
date: 2014-08-18 16:58:55 +0200
category: blog
description: "Create code snippets with Vim's built-in abbreviations feature."
---

Abbreviations are a pretty neat, and often ignored, feature of Vim. Similar to mappings, abbreviations are meant to be used in Insert mode, and allow you to replace some combination of letters with a word. This can be helpful for common typos or, as their name suggest, for abbreviating long words.

For instance, _approachable_ is both a relatively long word and I mistype it 80% of the time. With abbreviations I can issue the following command:

{% highlight vim %}
:iabbrev aprch approachable
{% endhighlight %}

Then, every time I type ``aprch`` and ``<SPACE>`` my typo will be replaced with the correct word. This is a pity example though, abbreviations can be extremely helpful for things like email addresses, end of letter sentences, salutations, etc.

However, in my case I use abbreviations as snippets for common programming definitions, like self invoking anonymous functions in JavaScript or spec declarations.

For instance, by typing ``iif<SPACE>`` Vim will replace my text with:

{% highlight js linenos %}
(function() {
  'use strict';

  // Cursor here, in Insert mode
})();
{% endhighlight %}

Such snippet-like functionality can be achieved by a simple combination of autocommand, key annotations, and abbreviations.

{% highlight vim linenos %}
autocmd FileType javascript :iabbrev <buffer> iif (function() {
      \<CR>'use strict';
      \<CR>
      \<CR>})();<ESC><s-O>
{% endhighlight %}

The above script will replace _iif_ with the function declaration in the current buffer, and only for javascript files. No external plugins needed!
