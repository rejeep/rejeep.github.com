---
title: Enclose and Wrap Region
layout: post
categories:
 - emacs
 - elisp
---

Wrap Region is an Emacs minor mode that wraps a region with
punctuations, much like in TextMate.

When no region is selected and a wrap key is pressed, Wrap Region
would insert both punctuations and place the cursor in between, also
much like in TextMate.

In my opinion, since I like to keep things to the point, Wrap Region
should do one job (wrap text) and do that good. That's why I decided
to split up Wrap Region into **Wrap Region** and **Enclose**.

## Wrap Region

<pre>Wraps selected text with punctuations or tag. If no region is selected, fall back.</pre>

Works like before, only that the double insertion of punctuations is gone.

Source: <http://github.com/rejeep/wrap-region>

## Enclose
<pre>Enclose cursor within punctuations. If region is selected, fall back.</pre>

Enclose mode was built from scratch, with inspiration taken from
TextMate. Enclose keeps a hash with keys that will enclose. Pressing
any of those keys will insert that key and its right buddy, and place
the cursor in between.

When cursor is in focus (not moved since inserting punctuations),
pressing **DEL** will remove both punctuations, and pressing the right
punctuation, will jump right over it.

See source for installation and usage examples.

Source: <http://github.com/rejeep/enclose>
