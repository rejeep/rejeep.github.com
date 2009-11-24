---
title: Testing User Privileges with Shoulda
layout: post
categories:
 - rails
 - test
 - refactor
---

It is really easy to test controllers with Shoulda. But testing a site
that have users, administrators, reporters, etc... is a pain if you
don't do a little bit of customization yourself. It's not hard to test
that a user has to be an admin to visit a page, but if the tests are
not kept DRY, this will end up in a big mess.

A while ago I used to do something like this to test that a visitor
must be logged in to visit a page.
{% highlight ruby %}
class PostsControllerTest < ActionController::TestCase

  context "on GET to :new" do
    context "when logged in" do
      setup do
        login_as_user

        get :new
      end

      should_assign_to :post
      # etc...
    end

    context "when not logged in" do
      setup { get :new }

      should_not_assign_to :post
      # etc...
    end
  end

end
{% endhighlight %}
This is nothing but **true pain**. It takes a long time write and the
tests are hard to read. Specially if the same thing is tested for all
REST actions, or more.

Finally, I got tired of testing like that and wrote this little piece
of code, that nowdays saves both my fingers and eyes:
{% highlight ruby %}
class ActiveSupport::TestCase

  def self.should_require_user(action, &block)
    context "#{action} when not logged in as user" do
      setup { instance_eval(&block) }

      should_respond_with :redirect
      should_redirect_to("the login path") { login_path }
      should_set_the_flash_to "You must be logged in to access this page"
    end
  end

  def self.should_require_no_user(action, &block)
    context "#{action} when logged in as user" do
      setup do
        login_as_user

        instance_eval(&block)
      end

      should_respond_with :redirect
      should_redirect_to("the root path") { root_path }
      should_set_the_flash_to "You must be logged out to access this page"
    end
  end

  def self.should_require_admin(action, &block)
    context "#{action} when logged in as a non admin user" do
      setup do
        login_as_user

        instance_eval(&block)
      end

      should_respond_with :redirect
      should_redirect_to("the login path") { login_path }
      should_set_the_flash_to "You must be an admin user to access this page"
    end
  end

end
{% endhighlight %}
Put this in a file named whatever you find appropriate and put it in
**test/shoulda_macros**. I call my file **security.rb**.

The code defines three methods, which are all obvious, but I show one
example of each below.

### should_require_user ###
Remake of the test above.
{% highlight ruby %}
class PostsControllerTest < ActionController::TestCase

  should_require_user("on GET to :new") { get :new }

  context "on GET to :new" do
    setup do
      login_as_user

      get :new
    end

    should_assign_to :post
    # etc...
  end

end
{% endhighlight %}

### should_require_no_user ###
User can not be logged in to visit the new action.

{% highlight ruby %}
class UserSessionsControllerTest < ActionController::TestCase

  should_require_no_user("on GET to :new") { get :new }

  context "on GET to :new" do
    setup { get :new }

    should_assign_to :user_session
    # etc...
  end

end
{% endhighlight %}

### should_require_admin ###
User must be an admin to visit the index action.

{% highlight ruby %}
class AdminControllerTest < ActionController::TestCase

  should_require_admin("on GET to :index") { get :index }

  context "on GET to :index" do
    context "as an admin" do
      setup do
        login_as_admin

        get :index
      end

      should_assign_to :secrets
      # etc...
    end
  end

end
{% endhighlight %}

That is **one** line of code to say that you must be logged in as a
user, an admin or not logged in at all to visit an action.

### But, is this code really DRY? ###
In all the examples, the exact same **get request** is specified at two
places. Luckely, Ruby lambdas can help refactor this.
{% highlight ruby %}
class AdminControllerTest < ActionController::TestCase

  request_get = lambda { get :index }
  
  should_require_admin "on GET to :index", &request_get

  context "on GET to :index" do
    context "as an admin" do
      setup { login_as_admin }
      setup &request_get

      should_assign_to :secrets
      # etc...
    end
  end

end
{% endhighlight %}

The only thing left that is not DRY, is **on GET to :index** . However,
this is not as easy to refactor as one might think. One option is to
change the shoulda extention and skip the first argument.
{% highlight ruby %}
class ActiveSupport::TestCase

  # ...

  def self.should_require_admin(&block)
    context "when logged in as a non admin user" do
      setup do
        login_as_user

        instance_eval(&block)
      end

      # ...
    end
  end

end
{% endhighlight %}

And then put the **should_require_admin** part, in the **on GET to :index**
context, like this.
{% highlight ruby %}
class AdminControllerTest < ActionController::TestCase

  request_get = lambda { get :index }
  
  context "on GET to :index" do
    should_require_admin &request_get

    context "as an admin" do
      setup { login_as_admin }
      setup &request_get

      should_assign_to :secrets
      # etc...
    end
  end

end
{% endhighlight %}
However, this will instead force you to repeat **as an admin** which
is my opinion is worse since you have to do it once for each action an
admin is required.
