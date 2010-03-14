---
title: Drag Stuff in Emacs
layout: post
categories:
 - emacs
 - elisp
---

I got a comment in my
[last post](http://blog.tuxicity.se/elisp/emacs/2010/03/11/duplicate-current-line-or-region-in-emacs.html)
regarding dragging a line up and down. I remembered that I once
started working on a minor mode "drag stuff". Whatever happened to
that I don't know.

Having a few hours to spare this weekend I decided to create the
mode. The mode is called drag-stuff-mode. The source is at
[Github](http://github.com/rejeep/drag-stuff).

## Installation
First download the source from
[Github](http://github.com/rejeep/drag-stuff)
and make sure it's in Emcacs load-path. Then require it. A more
detailed installation description is given in the README and lisp
source file.
    
## Usage
First off, start the mode with **M-x drag-stuff-mode**. Note that all
command takes a prefix argument to repeat the command that many times.

### Drag line
To drag a line, put the cursor on that line and press **<M-up>** or **<M-down>**.

For example, put the cursor on line 3 and press **<M-up>**.
{% highlight ruby linenos %}
class Foo
end
attr_accessor :bar
{% endhighlight %}

You should now have
{% highlight ruby linenos %}
class Foo
attr_accessor :bar
end
{% endhighlight %}

### Drag lines
Drag lines work like drag line except for that you can move more than
one line. Note that you don't have to select the complete lines. It's
enough that some part of the first and last lines are selected.

Mark the to_s method and press **<M-down>**.
{% highlight ruby linenos %}
def to_s
  ":#{bar}:"
end
class Foo
end
{% endhighlight %}

You should now have
{% highlight ruby linenos %}
class Foo
def to_s
  ":#{bar}:"
end
end
{% endhighlight %}

### Drag region
You can drag a region left and right by selecting a region and press
**<M-left>** or **<M-right>**.

For example. Select **{bar}** and press **<M-right>** until it's at
the correct place. You could also do this with by giving a prefix
argument like **M-3 M-right**.
{% highlight ruby linenos %}
class Foo
  def to_s
    {bar}":#:"
  end
end
{% endhighlight %}

You should now have
{% highlight ruby linenos %}
class Foo
  def to_s
    ":#{bar}:"
  end
end
{% endhighlight %}

### Drag word
To drag a word, just place the cursor on the word and press
**<M-left>** or **<M-right>**.

Put the cursor on **me** and press **<M-right>** a few times.
{% highlight ruby linenos %}
class Foo
  def to_s
    "drag me to the right"
  end
end
{% endhighlight %}

You should now have
{% highlight ruby linenos %}
class Foo
  def to_s
    "drag to the me right"
  end
end
{% endhighlight %}

There are obviously more things you can drag. These were what I came
up with. If you think that my selection is stupid and I should change
something, leave a comment with a good reason an I'll see what I can
do! ;)
