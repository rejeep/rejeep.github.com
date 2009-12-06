---
title: Emacs TAGS in Rails
layout: post
categories:
 - rails
 - emacs
---

If you do your Rails development in Emacs, you should use Emacs TAGS
for fast navigation. If you don't know what Tags tables are, read the
[Tags Tables](http://www.gnu.org/software/emacs/manual/html_node/emacs/Tags.html)
section in the Emacs manual. But, to give a short description, Tags
Tables can be used for fast lookup of for example a method definition.

This rake task creates a TAGS file of all ruby files in **lib** and
**app**, and all javascript files directly under
**public/javascripts**.

{% highlight ruby %}
module Tags
  RUBY_FILES = FileList["lib/**/*.rb", "app/**/*.rb"]
  JAVASCRIPT_FILES = FileList["public/javascripts/*.js"]
end

namespace :tags do
  desc "Creates an Emacs TAGS file"
  task :emacs do
    puts "Creating Emacs TAGS file..."
    sh "ctags -e -o TAGS #{Tags::RUBY_FILES} #{Tags::JAVASCRIPT_FILES}"
  end
end
{% endhighlight %}

Add a file named **tags.rake** in **lib/tasks** and then run **rake
tags:emacs** and a file named TAGS will end up in you Rails root.

Then all you have to do is place the cursor on a method call and press
**M-.** and you should be forwarded to that method definition.
