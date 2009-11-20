---
title: Share named scopes in Rails
layout: post
categories:
 - rails
 - dry
---

In a project I'm working on we have chemicals that belongs to a
department and a department has many chemicals:
{% highlight ruby %}
# app/models/chemical.rb
class Chemical < ActiveRecord::Base
  belongs_to :department
end

# app/models/department.rb
class Department < ActiveRecord::Base
  has_many :chemicals
end
{% endhighlight %}

In our project we use named scope extensively. This is an example:
{% highlight ruby %}
# app/models/chemical.rb
class Chemical < ActiveRecord::Base
  belongs_to :department

  named_scope :belongs_to_department, lambda { |department| { :conditions => { :department_id => department } } }
end
{% endhighlight %}

With this scope all chemicals that belongs to a department can be
fetched with:

{% highlight ruby %}
Chemical.belongs_to_departments(department)
{% endhighlight %}

But, chemicals was far from the only model belonging to a
department. People for example, were this scope also came in handy.
{% highlight ruby %}
# app/models/person.rb
class Person < ActiveRecord::Base
  belongs_to :department

  named_scope :belongs_to_department, lambda { |department| { :conditions => { :department_id => department } } }
end
{% endhighlight %}

Works fine, but now that same code is in two models. This way of doing
things is bad and we need to find a way to refactor this in order to
make our code
[DRY](http://en.wikipedia.org/wiki/Don%27t_repeat_yourself). So the
scope needs to be shared through all models that belongs to a
department (and needs that scope), and that with our code still DRY.

This is usually achived by including a module in the models, as it is
in this case. But since sharing a named scope is very common, I want
to have them nicely structured.

So in lib, I have a folder named named_scope. And in that folder, all
modules with common named scopes are. To get extra nice structure I
create a module called NamedScope:
{% highlight ruby %}
# lib/named_scopes/named_scope.rb
module NamedScope
end
{% endhighlight %}

Now I create the module that will hold the common department
scope. And that is done by using self.included:
{% highlight ruby %}
# lib/named_scopes/department.rb
module NamedScope::Departments
  def self.included(base)
    base.class_eval do
      named_scope :belongs_to_departments, lambda { |departments| { :conditions => { :department_id => departments } } }
    end
  end
end
{% endhighlight %}

So now in my models I can include that module:
{% highlight ruby %}
# app/models/chemical.rb
class Chemical < ActiveRecord::Base
  include NamedScope::Departments

  belongs_to :department
end

# app/models/person.rb
class Person < ActiveRecord::Base
  include NamedScope::Departments

  belongs_to :department
end
{% endhighlight %}

Now I can, with a DRY code and a nice structure, do this on both
models:
{% highlight ruby %}
Chemical.belongs_to_department(department)
Person.belongs_to_department(department)
{% endhighlight %}
