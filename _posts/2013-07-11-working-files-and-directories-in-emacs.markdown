---
title: Working files and directories in Emacs
layout: post
categories:
 - emacs
---

Much inspired by [@magnars](https://github.com/magnars)'s excellent
[s.el](https://github.com/magnars/s.el) and
[dash.el](https://github.com/magnars/dash.el),
[f.el](https://github.com/rejeep/f.el) is a modern API for working
with files and directories in Emacs.

## Example

{% highlight scheme %}
(let* ((foo-path
        (f-join "~" "tmp" "foo"))
       (foo-file
        (f-expand "file.txt" foo-path)))
  (unless (f-directory? foo-path)
    (f-mkdir foo-path))
  (f-write foo-file "SOME ")
  (f-write foo-file "CONTENT" 'append)
  (message
   "File '%s' have content '%s' and size '%d'"
   foo-file
   (f-read foo-file)
   (f-size foo-path)))
{% endhighlight %}

The output will be:

    File '~/foo/file.txt' have content 'SOME CONTENT' and size '12'
