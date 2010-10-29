---
title: ruby-end, automatically close ruby blocks
layout: post
categories:
 - emacs
 - elisp
 - ruby
---

In Ruby all blocks are closed with the **end** keyword. The Emacs mode
**ruby-electric** automatically inserts **end** when typing a block
keyword, followed by a space.

This is a nice feature, but unfortunately ruby-electric comes with
some other stuff, such as inserting **{}** and placing the cursor
between, when pressing **{**.

Since I already have that functionality from
[enclose-mode](https://github.com/rejeep/enclose), I don't want to use
ruby-electric, so I created a new mode **ruby-end** that does exactly
one thing and that is closing blocks.

The closing block functionality is a very simple task so creating a
function seemed best at first, but I decided to create a minor mode
**ruby-end**, because it's easier to test
(<https://github.com/rejeep/ruby-end/blob/master/features/ruby-end.feature>)
and you can turn it off.

Check it out at Github: <https://github.com/rejeep/ruby-end>
