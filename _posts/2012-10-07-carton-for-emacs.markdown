---
title: Carton for Emacs
layout: post
categories:
 - emacs
 - carton
 - elpa
---

This weekend, I finally had some time to work on Ecukes. Just when I
was about to start, I saw an
[issue](https://github.com/rejeep/package-spec/issues/1) by
[Joel McCracken](@joelmccracken) on my
[package-spec](https://github.com/rejeep/package-spec/issues/1)
project.

He said he had been looking at "modernizing" the emacs package
development stack, meaning that Emacs package developers should not
have to use git submodules to handle dependencies.

Emediately, he caught my attention. It's so easy to be a package
developer for Emacs and yet at the same time it's sooo hard and
annoying. Writing the code is easy, but distributing the package with
its dependencies is something else.

Anyway... No Ecukes work for me this weekend. Instead, I ended up
writing a new package, called
[carton](https://github.com/rejeep/carton). The goal is to make Emacs
dependency management made easy, or at least as easy as it can without
involving Emacs core.

It works for both your local Emacs installation and the packages you
are developing. All instructions are in the
[README](https://github.com/rejeep/carton#readme), but to give you an
idea, this is my set up from my local Emacs installation:

{% highlight scheme %}
(source "melpa" "http://melpa.milkbox.net/packages/")
(source "marmalade" "http://marmalade-repo.org/packages/")

(depends-on "carton")
(depends-on "wrap-region")
(depends-on "coffee-mode")
(depends-on "drag-stuff")
(depends-on "expand-region")
(depends-on "html-script-src")
(depends-on "htmlize")
(depends-on "magit")
(depends-on "projectile")
(depends-on "ruby-end")
(depends-on "ruby-tools")
;; ...
{% endhighlight %}

And for my [wrap-region](https://github.com/rejeep/wrap-region) project:

{% highlight scheme %}
(source "melpa" "http://melpa.milkbox.net/packages/")

(package "wrap-region" "0.6.0" "Wrap region with stuff.")

(development
 (depends-on "ecukes")
 (depends-on "espuds"))
{% endhighlight %}
