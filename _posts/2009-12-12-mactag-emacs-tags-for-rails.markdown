---
title: Mactag - Ctags for Rails
layout: post
categories:
 - rails
 - emacs
---


[Ctags](http://ctags.sourceforge.net/)
allows you to follow tags (of functions, variables, macros, whatever)
to their definitions. If you do your Rails development in an editor
that supports Ctags you might have been manually setting the TAGS-file
up in your projects. To create a TAGS-file in my Rails projects, I
always created a rake task.

Finally, I got tired of this and decided to write up a Rails plugin to
do it. The result is
[Mactag](https://github.com/rejeep/mactag).
Mactag gives you a DSL to create a TAGS-file for your Rails projects.

An example configuration (**config/mactag.rb**) might look like this:

{% highlight ruby %}
Mactag::Table.generate do
  app "app/**/*.rb", "lib/*.rb"

  plugins "thinking-sphinx", "whenever"

  gems "paperclip", "authlogic"
  gem "formtastic", :version => "0.9.7"

  rails :except => :actionmailer, :version => "2.3.5"
end
{% endhighlight %}

When this is set up you just run the builtin rake-task and the
TAGS-file will be created for you.
{% highlight bash %}
$ rake mactag
{% endhighlight %}

See
[README at Github](https://github.com/rejeep/mactag)
for a brief howto. And the
[documentation](http://rdoc.info/projects/rejeep/mactag)
for detailed information.
