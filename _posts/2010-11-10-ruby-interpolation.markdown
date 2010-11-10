---
title: Ruby interpolation
layout: post
categories:
 - emacs
 - elisp
 - ruby
---

In Ruby, you can in double quoted strings interpolate. For example:

{% highlight ruby %}
"1 + 2 = #{1 + 2}" # "1 + 2 = 3"
'1 + 2 = #{1 + 2}' # "1 + 2 = \#{1 + 2}" 
{% endhighlight %}

I made a function **ruby-interpolate** that inserts **#{}** and places
the cursor between **{** and **}** if the cursor is in a double quoted
string.

{% highlight scheme %}
(defun ruby-interpolate ()
  "In a double quoted string, interpolate."
  (interactive)
  (insert "#")
  (let ((properties (text-properties-at (point))))
    (when (and
           (memq 'font-lock-string-face properties)
           (save-excursion
             (ruby-forward-string "\"" (line-end-position) t)))
      (insert "{}")
      (backward-char 1))))
{% endhighlight %}

Bind the key:
{% highlight scheme %}
(define-key ruby-mode-map (kbd "#") 'ruby-interpolate)
{% endhighlight %}
