---
title: JS2-mode and Yasnippet
layout: post
categories:
 - emacs
 - javascript
 - js2-mode
 - yasnippet
---

**JS2-mode** for **Emacs** is a great mode. But it remaps some keys
that makes some behavior change. In my case it changed the **TAB** key,
and thereby disabled **Yasnippet**. To fix it, I added this to
**~/.emacs**:

{% highlight scheme %}
(eval-after-load 'js2-mode
  '(progn
     (define-key js2-mode-map (kbd "TAB") (lambda()
                                            (interactive)
                                            (let ((yas/fallback-behavior 'return-nil))
                                              (unless (yas/expand)
                                                (indent-for-tab-command)
                                                (if (looking-back "^\s*")
                                                    (back-to-indentation))))))))

{% endhighlight %}
