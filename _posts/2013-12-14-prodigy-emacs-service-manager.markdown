---
title: Prodigy - Emacs service manager
layout: post
categories:
 - emacs
---

I came up with the idea of
[Prodigy](https://github.com/rejeep/prodigy.el) when I got to work one
Monday morning and before I could start working I had to manually
start ten or so services. That means:

* Open ten tabs in the terminal
* Navigate to each project
* Remember how to start that service and do that
* Find the right tab when you want to check the logs for a service
* And more...

Prodigy provides a [Magit](https://github.com/magit/magit)-like GUI to
manage services in a simple way from Emacs. Now I can with a few
keystrokes close down all my services when I go home from work and
start them when I get to work.

Services are created using `prodigy-define-service`:

{% highlight scheme %}
(prodigy-define-service
  :name "Project X"
  :cwd "~/Code/project"
  :command "nodemon"
  :args '("app.coffee")
  :tags '(work test)
  :port 9001)
{% endhighlight %}

Start Prodigy using `M-x prodigy`. See more information at
https://github.com/rejeep/prodigy.el.
