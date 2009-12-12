---
title: Mactag - Emacs TAGS for Rails
layout: post
categories:
 - rails
 - emacs
---

If you do your programming in Emacs, you probably know that you can use
[Emacs Tags](http://www.gnu.org/software/emacs/manual/html_node/emacs/Tags.html)
to follow tags (of functions, variables, macros, whatever)
to their definitions. To create a TAGS-file for my Rails projects, I
always created a rake task.

Finally, I got tired of this and decided to write up a Rails plugin to
do it. The result is
[Mactag](https://github.com/rejeep/mactag). Mactag gives you a DSL to
create a TAGS-file for your Rails projects.

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
