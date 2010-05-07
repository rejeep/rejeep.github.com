---
title: Comment stuff in Emacs
layout: post
categories:
 - emacs
 - elisp
---

I find myself commenting a lot of stuff, so I made this little helper
function that either comments or uncomments the current line (if no
region is selected) or all lines in the selected region.

{% highlight scheme %}
(defun comment-or-uncomment-current-line-or-region ()
  "Comments or uncomments current current line or whole lines in region."
  (interactive)
  (save-excursion
    (let (min max)
      (if (region-active-p)
          (setq min (region-beginning) max (region-end))
        (setq min (point) max (point)))
      (comment-or-uncomment-region
       (progn (goto-char min) (line-beginning-position))
       (progn (goto-char max) (line-end-position))))))
{% endhighlight %}

My binding:
{% highlight scheme %}
(global-set-key (kbd "C-7") 'comment-or-uncomment-current-line-or-region)
{% endhighlight %}
