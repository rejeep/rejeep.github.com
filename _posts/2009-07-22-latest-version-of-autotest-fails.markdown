---
title: Latest version of Autotest fails
layout: post
categories:
 - autotest
 - testing
 - rails
---

The lastest version of **Autotest** introduced some trouble for be.

First I had trouble even starting Autotest. The solution was to
install **rails-autotest**:

{% highlight bash %}
sudo gem install rails-autotest
{% endhighlight %}

Then however I was having another trouble using **Redgreen**.
I got this error:
<pre>no such file to load -- autotest/redgreen (LoadError)</pre>

Turns out that the require in **.autotest** has taken a turn. Changing from:

{% highlight ruby %}
require 'autotest/redgreen'
{% endhighlight %}

to:
{% highlight ruby %}
require 'redgreen/autotest'
{% endhighlight %}

did the job.
