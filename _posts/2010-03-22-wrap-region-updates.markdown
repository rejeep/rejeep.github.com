---
title: Wrap region updates
layout: post
categories:
 - emacs
---

Actually, the
[first post](http://blog.tuxicity.se/emacs/2008/12/08/wrap-region-minor-mode-for-emacs.html)
I made in this blog was about the first minor mode I ever did,
[Wrap Region](http://github.com/rejeep/wrap-region/).
That was more than a year ago. I have learnt a lot since then and when
I looked through the source code for
[Wrap Region](http://github.com/rejeep/wrap-region/)
I found some stuff that needed a refreshment.

What I did was a lot of code cleanup and a bug fix for when wrapping
region with tag. I also added a global mode to activate wrap-region in
all buffers. The more notable change I did was that I added better
support for **"insert twice"**.

## Insert twice
If you set **wrap-region-insert-twice** to **t**, when no region is
selected and a punctuation key is hit, that key and its right buddy
will be inserted.

The updates in this version are:

  1. If you hit a punctuation key without having selected a region, hitting **DEL** will remove both the left and right punctuation.
  2. If you right after hitting the punctuation key, hit the right buddy to it, it will jump over the right one.

In both the above cases, this is only true if you hit a punctuation key
and don't move the cursor. As soon as your cursor moves, it is disabled.
