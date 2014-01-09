---
title: IE and MIME-types
layout: post
categories:
 - rails
 - ie
 - paperclip
---

I'm using **Paperclip** for a site where a visitor can upload an
image. The supported image types are **gif**, **jpg** and **png**.

So I created this Paperclip model validation:

{% highlight ruby %}
validates_attachment_content_type :image, :content_type => [ 'image/jpeg', 'image/png', 'image/gif' ]
{% endhighlight %}

But of course something had to not work in Internet Explorer. Not
obvious how a browser can affect something to fail on the server
side. It turns out that IE translates the MIME-types a bit different
than what other browsers do.

    FORMAT          MIME-IE          MIME-OTHER
    png             image/x-png      image/png
    jpeg            image/pjpeg      image/jpeg

So the "correct" Paperclip validation for gif, jpg and png images would then be:

{% highlight ruby %}
validates_attachment_content_type :image, :content_type => [ 'image/jpeg', 'image/png', 'image/gif', 'image/x-png', 'image/pjpeg' ]
{% endhighlight %}
