---
title: Command line parsing in Emacs
layout: post
categories:
 - emacs
---

[commander.el](https://github.com/rejeep/commander.el) is a command
line parser for Emacs. It avoids messing with `command-switch-alist`
(and friends) and instead defines the schema in an elegant API.

## Example schema

Here is a (silly) example where numbers can be added and subtracted.

{% highlight scheme %}
(require 'commander)

(defvar calc-fn nil)

(defun calc (&rest args)
  (if calc-fn
      (message "%s" (apply calc-fn (mapcar 'string-to-int args)))
    0))

(defun add ()
  (setq calc-fn '+))

(defun sub ()
  (setq calc-fn '-))

(commander
 (option "--help, -h" "Show usage information" 'commander-print-usage)
 (option "--add" "Add values" 'add)
 (option "--sub" "Subtract values" 'sub)
 (command "calc [*]" "Calculate these values" 'calc))
{% endhighlight %}

## Example usage

Add list of values

{% highlight bash %}
$ carton exec emacs --script math.el -- calc 1 2 3 4 5 --add

15
{% endhighlight %}

Subtract list of values

{% highlight bash %}
$ carton exec emacs --script math.el -- calc 1 2 3 4 5 --sub

-13
{% endhighlight %}

Show usage information

{% highlight bash %}
$ carton exec emacs --script math.el -- --help

USAGE: math.el COMMAND [OPTIONS]

COMMANDS:
 calc <*>            Calculate these values

OPTIONS:
 --sub               Subtract values
 --add               Add values
 -h                  Show usage information
 --help              Show usage information
{% endhighlight %}

More information is available at Github: <https://github.com/rejeep/commander.el>
