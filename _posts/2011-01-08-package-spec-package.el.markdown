---
title: Package spec for package.el
layout: post
categories:
 - emacs
 - elisp
 - elpa
---

[Package spec](https://github.com/rejeep/package-spec)
 is a simple DSL for **package.el** where you can specify sources and
packages. Package spec is based on code from
[starter-kit-elpa.el](https://github.com/technomancy/emacs-starter-kit/blob/master/starter-kit-elpa.el)
in the Emacs starter kit.

To use package-spec, simply put package-spec in Emacs load-path and
require it.

{% highlight cl %}
(add-to-list 'load-path "/path/to/package-spec")
(require 'package-spec)
{% endhighlight %}

Once installed, create a file in **~/.emacs.d** called
**package-spec.el**. In that file you can use to functions: **source**
and **package**. For example:

{% highlight cl %}
(source "elpa" "http://tromey.com/elpa/")
(source "marmalade" "http://marmalade-repo.org/packages/")
 
(package "drag-stuff")
(package "wrap-region")
(package "ruby-end")
(package "magit")
{% endhighlight %}

Then just restart Emacs and those packages will be installed and
loaded automatically.

Source: <https://github.com/rejeep/package-spec>
