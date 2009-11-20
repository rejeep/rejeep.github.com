---
title: Emacs JDE completion with ido and yasnippet
layout: post
categories:
 - emacs
 - java
 - jde
---

I love all kind of completion. But I don't like JDE's built in
completion methods. I therefore wrote a little function that uses two
of my favorite tools in Emacs: [IDO](http://www.cua.dk/ido.html) and
[Yasnippet](http://code.google.com/p/yasnippet).

Put the defun in the JDE hook.
{% highlight scheme %}
(defun jde-complete-ido ()
  "Custom method completion for JDE using ido-mode and yasnippet."
  (interactive)
  (let ((completion-list '()) (variable-at-point (jde-parse-java-variable-at-point)))
    (dolist (element (jde-complete-find-completion-for-pair variable-at-point nil) nil)
      (add-to-list 'completion-list (cdr element)))
    (if completion-list
        (let ((choise (ido-completing-read "> " completion-list nil nil (car (cdr variable-at-point)))) (method))
          (unless (string-match "^.*()$" choise)
            (setq method (replace-regexp-in-string ")" "})"(replace-regexp-in-string ", " "}, ${" (replace-regexp-in-string "(" "(${" choise)))))
          (delete-region (point) (re-search-backward "\\." (line-beginning-position)))
          (insert ".")
          (if method
              (yas/expand-snippet (point) (point) method)
            (insert choise)))
      (message "No completions at this point"))))
{% endhighlight %}

Eval and do C-c C-v . on a variable.
{% highlight java %}
public class Test
{
  public static void main(String[] args)
  {
    String mess = "Hello World!";
    mess.sub[CURSOR HERE]
  }
}
{% endhighlight %}

That should result in the following in the minibuffer:
{% highlight bash %}
> {substring(int, int) | substring(int) | subSequence(int, int)}
{% endhighlight %}

Due to lack of good design in jde-complete.el (if you ask me) this
defun will not handle all cases of completion. Even though it handles
the most important once. In order to cover all cases lot of code from
functions in jde-complete.el had to be copied, which I didn't want to
do.
