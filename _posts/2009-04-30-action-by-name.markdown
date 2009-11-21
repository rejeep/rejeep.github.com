---
title: Action By Name
layout: post
categories:
 - java
 - rails
---

How many blogs have I read lately, where the authors first words are:
"It's been a while since my last post". I can not do anything other
than the same, because it has been a while.

To the point. I'm working on a Java project in school right
now. Coming from a long time development in Rails, I got tired of all
the typing and asked myself: Where did all the "It just works" go? And
I came to the conclusion that there is none, or very little. So I took
matters in to my own hands and created a somewhat Rails like
functionality for controllers.

As all Rails developers know, or at least should know, the controller
is determined by **params[:controller]** and the action by
**params[:action]**. So what I did was something similar, only that you in
Java can not pass params, as you can in Rails.

### By name ###
Basically how it works is that when you create a controller you extend
**ApplicationController**, that in turn extend **ActionController**. Example
with one save method (or action).

{% highlight java %}
public class SomeController extends ApplicationController
{
  public void save()
  {
    // Do the saving...
  }
}
{% endhighlight %}

Then say we want to call that method when a button is pressed. By
extending **ApplicationController** it is as simple as this:

{% highlight java %}
JButton save = new JButton("Save");
save.addActionListener(someController);
save.setName("save");
{% endhighlight %}

Almost like in Rails. You specify controller and action, and then
Rails does the rest. Now when you click the save button, the method
save in **SomeController** will be called automatically. Easy, right? Or,
at least as easy as it can be in Java.

### Method Missing ###
After a while I was assigned a task and doing it I discovered some
limitations. The task was to create a menu, in which you could switch
between all different languages. Adding a new language to the
application is as easy as putting a **YAML-file** in a directory (À la
Rails). So I created the menu dynamically from all yml-files. I also
created a controller named **LanguageController**, that should receive
actions when the language should change.

I discovered that to make this happen, I had to create one method in
the controller for each locale (sv, en, fi, etc...). And that was
clearly not an option. So in **ActionController** I added some new
functionality. Most of you Ruby and Rails developers know it. I'm
talking about **method-missing**.

How it works is simple. If there's no method in the controller, with
the same name as the component, the method **methodMissing** in
**ActionController** is called. So in my case it was as simple as in
**LanguageController** override **methodMissing** and implement the language
switching.

### Interesting ###
If you want to know more. Read the few lines of code and it's many
lines of comments. It's at
[Github](http://github.com/rejeep/action_by_name). There's also a
simple example included.
