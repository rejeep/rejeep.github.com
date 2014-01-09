---
title: Shoulda requires a database record
layout: post
categories:
 - rails
 - shoulda
 - testing
---
I fell into a trap today when creating some tests using Shoulda.

I had a user model and for that a test. In the user model I had this
validation:

{% highlight ruby %}
validates_uniqueness_of :email
{% endhighlight %}

and in the test this:
{% highlight ruby %}
should_validate_uniqueness_of :email
{% endhighlight %}

But weird enough I got this failure:

    1) Failure:
    test: User should require case sensitive unique value for email. (UserTest)
    [/usr/lib/ruby/gems/1.8/gems/thoughtbot-shoulda-2.10.1/lib/shoulda/assertions.rb:50:in `assert_accepts'
    /usr/lib/ruby/gems/1.8/gems/thoughtbot-shoulda-2.10.1/lib/shoulda/active_record/macros.rb:88:in `__bind_1243608769_500830'
    /usr/lib/ruby/gems/1.8/gems/thoughtbot-shoulda-2.10.1/lib/shoulda/context.rb:253:in `call'
    /usr/lib/ruby/gems/1.8/gems/thoughtbot-shoulda-2.10.1/lib/shoulda/context.rb:253:in `test: User should require case sensitive unique value for email. ']:
    Can't find first User

I even got it when I changed the **:case_sensitive** option.

Turns out that **Shoulda** requires there to be a record in the
database. So by just creating a record I got rid of the failures:

{% highlight ruby %}
Factory(:user)
{% endhighlight %}

## Update
In recent versions of Shoulda this is solved by defining a **subject** block:

{% highlight ruby %}
class UserTest < ActiveSupport::TestCase
  subject { Factory(:user) }

  should_validate_uniqueness_of :email
end
{% endhighlight %}
