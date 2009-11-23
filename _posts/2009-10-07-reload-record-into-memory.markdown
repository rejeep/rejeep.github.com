---
title: Reload record into memory
layout: post
categories:
 - rails
 - shoulda
 - test
---

Lets say we are creating a simple blog. The blog has posts and each
post can have many comments:

{% highlight ruby %}
class Post < ActiveRecord::Base
  has_many :comments
end
 
class Comment < ActiveRecord::Base
  belongs_to :post
end
{% endhighlight %}

Lets also say that on the posts index page, all posts should be
listed, and for each post the number of comments. To make this
efficient lets add **counter cache** in posts for the number of
comments. First a **comments_count:integer** column must be added in the
posts table, and then the comments model has to be updated like this:

{% highlight ruby %}
class Comment < ActiveRecord::Base
  belongs_to :post, :counter_cache => true
end
{% endhighlight %}

Now to the interesting part. Lets create a test for it:

{% highlight ruby %}
context "a post" do
  setup { @post = Factory(:post) }
 
  context "gets a new comment" do
    setup { @comment = Factory(:comment, :post => @post) }
 
    should_change("the post comments count", :by => 1) { @post.comments_count }
   end
end
{% endhighlight %}

So, If we have a post. And then create a comment to that post. Then
the post **comments_count** should increase by one. This seems like a
good test. But it will fail because the **comments_count** was not
expected to be zero, but was. Why is that? Testing this in the console
will make the **comments_count** update correct.

Thinking about it, it’s kinda obvious why it fails. This is what
happens in the test: A post is created and then fetched from the
database into memory. Then the same thing happens with a comment. When
the comment record is saved, it makes sure that the post
**comments_count** is updated in the database. Thats right, in the
**database**. Above I said that a post was read into **memory**. So the post
the test is working with, is in memory. But the comment only updates
the post in the database.

So what needs to be done is to update the post from the database,
before checking the **comments_count**. Thankfully there’s a nice method
in **ActiveRecord** that helps with this, called **reload**. Lets update the
test to use reload, and then run it again. And what do you know, it
passes. Here’s the working test:

{% highlight ruby %}
context "a post" do
  setup { @post = Factory(:post) }
 
  context "gets a new comment" do
    setup { @comment = Factory(:comment, :post => @post) }
 
    should_change("the post comments count", :by => 1) { @post.reload.comments_count }
  end
end
{% endhighlight %}

So whenever you update a record from another. Make sure to reload
the record from the database to get the changes in to memory.
