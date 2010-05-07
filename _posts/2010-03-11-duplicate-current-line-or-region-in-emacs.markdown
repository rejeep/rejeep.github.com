---
title: Duplicate current line or region in Emacs
layout: post
categories:
 - emacs
 - elisp
---

What don't you do to avoid studying for an exam? I found myself bored
and wrote an Emacs Lisp function that duplicates the current line or
region. The are probably lots of those out there, but here's my
version of it.

{% highlight scheme %}
(defun duplicate-current-line-or-region (arg)
  "Duplicates the current line or region ARG times.
If there's no region, the current line will be duplicated. However, if
there's a region, all lines that region covers will be duplicated."
  (interactive "p")
  (let (beg end (origin (point)))
    (if (and mark-active (> (point) (mark)))
        (exchange-point-and-mark))
    (setq beg (line-beginning-position))
    (if mark-active
        (exchange-point-and-mark))
    (setq end (line-end-position))
    (let ((region (buffer-substring-no-properties beg end)))
      (dotimes (i arg)
        (goto-char end)
        (newline)
        (insert region)
        (setq end (point)))
      (goto-char (+ origin (* (length region) arg) arg)))))
{% endhighlight %}

Then bind a key to it. I use **C-c d**.
{% highlight scheme %}
(global-set-key (kbd "C-c d") 'duplicate-current-line-or-region)
{% endhighlight %}

There are a few ways to use the function.

## Duplicate line
Lets say you have this contents and you want to duplicate the second line.
{% highlight ruby linenos %}
class Foo
  attr_accessor :bar
end
{% endhighlight %}
Put your cursor at line **2** and press **C-c d**. You should now have this:
{% highlight ruby linenos %}
class Foo
  attr_accessor :bar
  attr_accessor :bar
end
{% endhighlight %}

## Duplicate region
Not lets try to duplicate a region.
{% highlight ruby linenos %}
class Foo
  def to_s
    ":#{bar}:"
  end
end
{% endhighlight %}

Select the lines **2** to **4**. Note that you don't have to select the
complete lines. It's enough that some part of the first and last lines
are selected.

Now press **C-c d** and the whole **to_s** method should be duplicated
like this:
{% highlight ruby linenos %}
class Foo
  def to_s
    ":#{bar}:"
  end
  def to_s
    ":#{bar}:"
  end
end
{% endhighlight %}

## Duplicate several times
Imagine you want to duplicate some line more than once. You could
hit **C-c d** multiple times, but there's a better way to do it.
{% highlight html linenos %}
<ul id="stuff-to-do">
  <li>Clean the house</li>
</ul>
{% endhighlight %}

Put the cursor on line **2** and press (depending on how many duplicates
you want) for example: **M-5 C-c d** or **C-u 10 C-c d**.

{% highlight html linenos %}
<ul id="stuff-to-do">
  <li>Clean the house</li>
  <li>Clean the house</li>
  <li>Clean the house</li>
  <li>Clean the house</li>
</ul>
{% endhighlight %}
