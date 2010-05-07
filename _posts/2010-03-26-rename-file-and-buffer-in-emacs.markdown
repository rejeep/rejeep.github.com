---
title: Rename file and buffer in Emacs
layout: post
categories:
 - emacs
 - elisp
---

In the past, when I was in a file that I wanted to rename, I either
opened up a console or dired to rename it. I never thought about
creating a function to make it simpler. But then I came across
[Steve Yegge's Emacs conf](http://sites.google.com/site/steveyegge2/my-dot-emacs-file)
and found a function that did just that. 

The function below is based on the one in Steve Yegge's Emacs conf. But
there are a few improvements to it:

1. File and directory completion
2. Can move file to other directory
3. Handles the case if Emacs is visiting a file and that file is removed

{% highlight scheme %}
(defun rename-file-and-buffer ()
  "Renames current buffer and file it is visiting."
  (interactive)
  (let ((name (buffer-name))
        (filename (buffer-file-name)))
    (if (not (and filename (file-exists-p filename)))
        (message "Buffer '%s' is not visiting a file!" name)
      (let ((new-name (read-file-name "New name: " filename)))
        (cond ((get-buffer new-name)
               (message "A buffer named '%s' already exists!" new-name))
              (t
               (rename-file filename new-name 1)
               (rename-buffer new-name)
               (set-visited-file-name new-name)
               (set-buffer-modified-p nil)))))))
{% endhighlight %}

I bound **C-c r** to the function:
{% highlight scheme %}
(global-set-key (kbd "C-c r") 'rename-file-and-buffer)
{% endhighlight %}
