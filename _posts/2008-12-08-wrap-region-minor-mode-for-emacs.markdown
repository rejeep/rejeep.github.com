---
title: wrap-region minor mode for Emacs
layout: post
categories:
 - emacs
---

I finally took the time to convert my wrap-region function and made a
minor mode of it. So why a mode and not just some function? Well... I
discovered the need to fast and easy be able to turn the bindings on
and off. And a minor mode seemed like a good solution.

wrap-region is a way to wrap contents with punctuations, rather than
just insert them. It also has the extra feature to wrap contents with
a markup tag:
{% highlight html %}
<tag attribute="...">...</tag>
{% endhighlight %}

Many lines of comments are included in the code, so I don't spend any
time repeating myself here.

Check it out at [Github](http://github.com/rejeep/wrap-region/)
