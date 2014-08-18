---
layout: post
title: "Multi-line abbreviations in Vim"
date: 2014-08-18 16:58:55 +0200
comments: true
description: Implementing multi-line abbreviations in Vimscript
categories:
---

Abbreviations are one of the nicest features of Vim. Very similar to mappings, abbreviations are meant to be used in insert mode instead, and essentially allow you to replace some combination of letters with a word.

For instance, _approachable_ is a word I end up mistyping 80% of the time, with abbreviations I can issue the following command:

``
:iabbrev aprch approachable
``

Then, every time I type _aprch_ followed by ``<SPACE>`` my typo will be replaced with the correct word. Perhaps more useful abbreviations can also be defined: email addresses, end of letter sentences, salutations, you name it.

However, I mainly use abbreviations for common programming definitions, like self invoking anonymous functions in JavaScript or spec declarations.

For instance, by typing ``iif<SPACE>`` I get:

```javascript
(function() {
  'use strict';

  // Cursor here, in Insert mode
})();
```

Which can be achieved by a simple combination of autocommand, key annotations, and abbreviations.

```bash
# Replace `iif` with function declaration, for the current buffer, only for .js files
autocmd FileType javascript :iabbrev <buffer> iif (function() {
      \<CR>'use strict';
      \<CR>
      \<CR>})();<ESC><s-O>
```

No external plugins needed!
