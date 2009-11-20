---
title: Send inline attachments in Rails
layout: post
categories:
 - attachment
 - email
 - image
 - logo
 - rails
---

I was looking for a way to send inline attachments in emails. This way
you can for example add your logo to an email.

Unfortunately there seems to be no easy way of doing this. After some
googleing I found a [ticket](http://dev.rubyonrails.org/ticket/2179)
for this with a patch that I got working.

For some reason the core developers did not accept the patch. So I
decided to make a plugin of it so that I easy could use it in my own
applications. The
[plugin](http://github.com/rejeep/inline_attachment/tree/master) is
available at Github if you have the same problem.
