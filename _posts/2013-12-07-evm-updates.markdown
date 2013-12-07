---
title: Evm updates
layout: post
categories:
 - emacs
 - testing
 - evm
---

[Evm](https://github.com/rejeep/evm) is a project that makes it
possible to install multiple Emacs versions at the same time, much
like [Rvm](http://rvm.io/) for Ruby. Evm has been around for a while
and it has always been a great idea, but not a great project. I
finally took the time to work on it. I rewrote it in Ruby (version 1.8
is supported, so most platforms should work out of the box).

The new version of Evm includes pre compiled binaries. These are
minimal installations, built for the sole purpose of testing Emacs
packages. If you run your package tests on Travis, you most likely
install the different Emacs versions via `apt-get`. I did that too,
until now. For example, this is the Travis configuration for
[f.el](https://github.com/rejeep/f.el):

{% highlight yaml %}
language: emacs-lisp
before_install:
  - curl -fsSkL https://gist.github.com/rejeep/7736123/raw | sh
  - export PATH="/home/travis/.cask/bin:$PATH"
  - export PATH="/home/travis/.evm/bin:$PATH"
  - evm install $EVM_EMACS --use
  - cask
env:
  - EVM_EMACS=emacs-23.4-bin
  - EVM_EMACS=emacs-24.1-bin
  - EVM_EMACS=emacs-24.2-bin
  - EVM_EMACS=emacs-24.3-bin
script:
  - emacs --version
  - make test
{% endhighlight %}

There are still a lot features that will be implemented in the
future. Check out the
[issues list](https://github.com/rejeep/evm/issues) for more
information.
