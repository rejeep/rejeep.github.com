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
(let* ((dir
        (f-join "~" "tmp" "dir"))
       (foo-file
        (f-expand "foo.txt" dir))
       (bar-file
        (f-expand "bar.txt" dir)))
  (unless (f-directory? dir)
    (f-mkdir dir))
  (f-write foo-file "SOME ")
  (f-write foo-file "CONTENT" 'append)
  (f-write bar-file "MORE...")
  (-map
   (lambda (file)
     (message
      "File: %s, content: '%s', size: %d"
      (f-relative file dir)
      (f-read file)
      (f-size file)))
   (f-files dir))
  (message "Total size of all files in %s: %d" dir (f-size dir)))
{% endhighlight %}

The output will be:

    File: bar.txt, content: 'MORE...', size: 7
    File: foo.txt, content: 'SOME CONTENT', size: 12
    Total size of all files in ~/tmp/dir: 19
