---
title: Ansi for Emacs
layout: post
categories:
 - emacs
 - elisp
 - ansi
---

When you create shell scripts, it can be quite nice with colored
output. It happens that I create shell scripts using Emacs batch
mode. To get colored output in these scripts, I created a library
called
[ansi.el](https://github.com/rejeep/ansi)
that takes a string and applies some kind of ansi effect to it.

Example of Emacs batch script that uses ansi.el:

{% highlight cl %}
#!/usr/bin/env emacs --script

(require 'ansi)

;; Do something

(message
 (if success
     (ansi-green "Your task was successfully completed!")
   (ansi-red "Something went wrong!")))
{% endhighlight %}

In cases where a lot of different colors are mixed it can be quite
cumbersome to write the **ansi-** prefix all over. Using the
**with-ansi** block, the prefix can be skipped. For example:

{% highlight cl %}
#!/usr/bin/env emacs --script

(require 'ansi)

(with-ansi
 (message "  |\\_/|  ")
 (message " / %s %s \\ " (blue "@") (green "@"))
 (message "( > %s < )" (blink (yellow "º")))
 (message " `»»x««´ ")
 (message " /  %s  \\ " (red "O")))
{% endhighlight %}

Yes, it will print a cat with one blue eye, one green eye, a blinking
yellow nose and a red button.

Source: <https://github.com/rejeep/ansi>
