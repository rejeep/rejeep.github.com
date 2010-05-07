---
title: Clean up buffer or region in Emacs
layout: post
categories:
 - emacs
 - elisp
---

Before I commit code I want to: Replace all tabs with spaces, make
sure indentation is correct and delete trailing whitespace.

Here's a function that does just that (inspired by
**cleanup-buffer** from
[Emacs starter kit](http://github.com/technomancy/emacs-starter-kit)).

{% highlight scheme %}
(defun clean-up-buffer-or-region ()
  "Untabifies, indents and deletes trailing whitespace from buffer or region."
  (interactive)
  (save-excursion
    (unless (region-active-p)
      (mark-whole-buffer))
    (untabify (region-beginning) (region-end))
    (indent-region (region-beginning) (region-end))
    (save-restriction
      (narrow-to-region (region-beginning) (region-end))
      (delete-trailing-whitespace))))
{% endhighlight %}

The function either operates on a region (if any region is selected)
or the whole buffer.

My binding:
{% highlight scheme %}
(global-set-key (kbd "C-c n") 'clean-up-buffer-or-region)
{% endhighlight %}

