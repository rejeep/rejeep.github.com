---
title: Wrap region 0.4.0
layout: post
categories:
 - emacs
 - elisp
---

I just updated [wrap region](https://github.com/rejeep/wrap-region) to
version **0.4.0**. The update includes two important changes: **custom
trigger key** and **mode specific wrappers**.

## Custom trigger key
When you add a wrapper, say **(** and **)**, the left punctuation will
be used as the trigger key. Now you can add wrappers with a custom
trigger key. For example, lets say you want **p** to wrap with the
p-tag. Then you can do this:

{% highlight scheme %}
(wrap-region-add-wrapper "<p>" "</p>" "p")
{% endhighlight %}

This feature is implemented by [tkf](https://github.com/tkf), thanks!

## Mode specific wrappers

The most notable feature though is mode specific wrappers. This allow
you to specify wrappers for specific modes. The docstring for
**wrap-region-add-wrapper** now looks like this:

{% highlight scheme %}
(wrap-region-add-wrapper LEFT RIGHT &optional KEY MODE-OR-MODES)
{% endhighlight %}

An example usage is the wrapper above. That wrapper only makes sense
in html-mode, which can be specified.

{% highlight scheme %}
(wrap-region-add-wrapper "<p>" "</p>" "p" 'html-mode)
{% endhighlight %}

Now **p** will only wrap in html-mode.

Lets say you want **#** to wrap with **#** and **#** in all modes
except css-mode and java-mode, then **#** should wrap with a comment
(**/\*** and **\*/**).

{% highlight scheme %}
(wrap-region-add-wrapper "#" "#")
(wrap-region-add-wrapper "/*" "*/" "#" '(css-mode java-mode))
{% endhighlight %}

This feature is implemented by [me](https://github.com/rejeep) and
[tkf](https://github.com/tkf), thanks again! :)

Check out the [README](https://github.com/rejeep/wrap-region#readme)
for more information.
