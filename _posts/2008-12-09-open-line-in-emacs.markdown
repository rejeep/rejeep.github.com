---
title: Open line in Emacs
layout: post
categories:
 - emacs
---

All the time when I'm programming, I find myself in the situation where
I'm in the middle of a line and want to open a line above or
below. Doing this would be accomplished with something like this:

In the above case: **C-a, C-m, C-p, &lt;tab&gt;**. And in the below case: **C-e, C-m, &lt;tab&gt;**

That's a lot of typing for such common tasks. So I created these
functions that do exactly what I describe. I also bound a key to each
function to have my lines opened much faster.

{% highlight scheme %}
(defun open-line-above ()
  "Open a line above the line the point is at.
Then move to that line and indent accordning to mode"
  (interactive)
  (move-beginning-of-line 1)
  (newline)
  (previous-line)
  (indent-according-to-mode))
{% endhighlight %}

{% highlight scheme %}
(defun open-line-below ()
  "Open a line below the line the point is at.
Then move to that line and indent accordning to mode"
  (interactive)
  (move-end-of-line 1)
  (newline)
  (indent-according-to-mode))
{% endhighlight %}

This could also be done with macros, but I don't really like them in these situations.

Bind keys to the functions like this, only replace ... with your desired keys:
{% highlight scheme %}
(global-set-key (kbd "...") 'open-line-above)
(global-set-key (kbd "...") 'open-line-below)
{% endhighlight %}
