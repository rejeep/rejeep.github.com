---
title: Testing in Emacs
layout: post
categories:
 - emacs
 - cask
 - ecukes
 - ert-runner
---

I started using Emacs about seven years ago. At that point, there was
one Emacs blog, but in the last two - three years, something has
happened. There are tons of blogs, screencasts and packages out
there. Very exciting times ahead, indeed!

At that point, basically no package was tested and yet somehow, it
worked quite well in practice. How this is possible is beoyned me?
Maybe Emacs developers are smarter than the average
programmer. Nevertheless, that is still no reason not to test your
Emacs packages.

How is it that [Magit](https://github.com/magit/magit), probably the
most awesome Emacs package ever, fixes bugs on a daily basis, but at
the same time introduces new ones at daily basis as well? No offence
to the Magit developers, they are doing a great job! I have talked to
them and they are aware that the project needs testing. Sure Emacs is
just an "editor", but the fact still remains that I use Emacs every
day for work and every day for spare time hacking. It is very annoying
when a package breaks, specially because of lack of tests.

Don't get me wrong. It is ok that packages break, but with testing
this can so easily be avoided. And the most important part, is that there
are no regressions. Testing Emacs packages these days is really easy,
which I will show in these upcomming posts. I will show how to unit
test and also how to integration test your packages.

The next post will be about unit testing!
