---
title: Save your Emacs finger before it's too late
layout: post
categories:
 - emacs
---

I'm serious! Mine almost died! I am of course talking about the left
[little finger](http://en.wikipedia.org/wiki/Little_finger). Do
yourself a favor and save your finger (and be faster, which is the
meaning of life) by switching Ctrl and Caps Lock. Put this in
~/.xmodmap:

{% highlight bash %}
remove Lock = Caps_Lock
remove Control = Control_L

keysym Control_L = Caps_Lock
keysym Caps_Lock = Control_L

add Lock = Caps_Lock
add Control = Control_L
{% endhighlight %}

And then put this line in ~/.bash_profile:
{% highlight bash %}
xmodmap ~/.xmodmap
{% endhighlight %}

I takes a while getting used to. But it cant be worse than learning to
use C-n, C-p, C-f, C-b, M-f, M-b, etc.. instead of using the arrows,
which you do use, right?
