---
title: Google region in Emacs
layout: post
categories:
 - emacs
---

## Update START
I recommend that you use my new version of this function that either
googles a region (if any selected) or a query.

{% highlight scheme %}
(defun google ()
  "Googles a query or region if any."
  (interactive)
  (browse-url
   (concat
    "http://www.google.com/search?ie=utf-8&oe=utf-8&q="
    (if mark-active
        (buffer-substring (region-beginning) (region-end))
      (read-string "Query: ")))))
{% endhighlight %}
## Update END

Two of my most frequently used tools when programming are **Emacs** and
**Firefox**. And much of the time in Firefox I spend on **Google**. That's why
I created this (simple) function that googles a region. Just select
the region you want to google and then do **M-x google-region** (or
preferably bind a key to it).

{% highlight scheme %}
(defun google-region (beg end)
  "Google the selected region."
  (interactive "r")
  (browse-url (concat "http://www.google.com/search?ie=utf-8&oe=utf-8&q=" (buffer-substring beg end))))
{% endhighlight %}

You must also set your browser:
{% highlight scheme %}
(setq browse-url-browser-function 'browse-url-generic
  browse-url-generic-program "/usr/bin/firefox")
{% endhighlight %}

To bind a key to the function (replace ... with you prefered key):
{% highlight scheme %}
(global-set-key (kbd "...") 'google-region)
{% endhighlight %}
