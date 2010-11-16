---
title: Delete file and buffer in Emacs
layout: post
categories:
 - emacs
 - elisp
---

Here's a handy function that kills the current buffer and removes the
file it is connected to.

{% highlight scheme %}
(defun delete-this-buffer-and-file ()
  "Removes file connected to current buffer and kills buffer."
  (interactive)
  (let ((filename (buffer-file-name))
        (buffer (current-buffer))
        (name (buffer-name)))
    (if (not (and filename (file-exists-p filename)))
        (error "Buffer '%s' is not visiting a file!" name)
      (when (yes-or-no-p "Are you sure you want to remove this file? ")
        (delete-file filename)
        (kill-buffer buffer)
        (message "File '%s' successfully removed" filename)))))
{% endhighlight %}

I bound **C-c k** to the function:
{% highlight scheme %}
(global-set-key (kbd "C-c k") 'delete-this-buffer-and-file)
{% endhighlight %}
