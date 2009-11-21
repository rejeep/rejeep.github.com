---
title: Replace newlines with commas
layout: post
categories:
 - bash
 - mysql
 - rails
 - replace
 - ruby
 - sql
 - tr
---

### Background ###

**Note!** If you're only interested in the solution, read the **Solution**
section at the end.

I had a **companies** table with more than 2M rows. And for each company I
wanted to set a column, **should_index**, based on some conditions. There
was no way I could write this in pure **SQL** (at least not with my
knowledge), so I created a simple **rake task**:

{% highlight ruby %}
namespace :companies do
  task :update_should_index => :environment do
    Company.find_in_batches do |companies|
      # ...
 
    end
  end
end
{% endhighlight %}

Running this took forever, so I wanted to avoid doing this again. I
knew that the companies table was not going to change, so I thought
I'd create a SQL-script and run it on the other environments and
computers that was working on the project. So I wanted to dump all
company ids that had **should_index = 1**:

{% highlight bash %}
$ echo "SELECT id FROM companies WHERE should_index = 1;" | mysql -u user -p -D database > ids.sql
{% endhighlight %}

Then all I had to do was to create a simple update query like this
with the ids in the file:

{% highlight sql %}
> UPDATE companies SET should_index = 1 WHERE id IN (1, 2, ... , n);
{% endhighlight %}

Pretty simple! Except that the **ids.sql** file contained more than **100 000**
lines of ids. **Emacs** usually is really good at these kind of
things, except that 100 000 lines was to much. So I had to figure out
a way to convert from:

<pre>
2334
45345
2342
23434
...
</pre>

To:

<pre>2334, 45345, 2342, 23434, ...</pre>

### Solution ###

I tried a little bit of **sed** and **awk**. But it turns out that the winner
by far was **tr**:

{% highlight bash %}
$ tr '\n' ', ' < ids.sql > ids_with_commas.sql
{% endhighlight %}

Thats it! Now I have a nice SQL-script that takes just a few seconds to run.
