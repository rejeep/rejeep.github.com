---
title: Various testing tools in Emacs
layout: post
categories:
 - emacs
 - cask
 - ert
 - ert-runner
 - ert-async
 - ecukes
 - testing
 - travis
---

I promised a last post in the _"testing in Emacs"_ series. I have
covered both unit and integration testing. In this post I will show
various tools you can use to improve your testing skills even more.

## Makefile

All my projects has a `Makefile` with tasks for running the tests and
setting up the local development environment. I recommend that you do
the same. Here is an example `Makefile`.

{% highlight makefile %}
CASK ?= cask
EMACS ?= emacs

all: test

test: unit ecukes

unit:
	${CASK} exec ert-runner

ecukes:
	${CASK} exec ecukes

install:
	${CASK} install

.PHONY:	all test unit ecukes install
{% endhighlight %}

In your projects README file, you can now under the _"Contribution"_
section write:

To contribute to my project, Install [Cask](http://cask.github.io/) and then:

{% highlight bash %}
$ make install
{% endhighlight %}

Run the tests with:

{% highlight bash %}
$ make test
{% endhighlight %}

## Travis

Travis is awesome! It's free for open source projects and really easy
to set up. So go ahead and do it for your projects now!

Here is a Travis configuration file that I use for all my projects:

{% highlight yaml %}
language: emacs-lisp
before_install:
  - curl -fsSkL https://gist.github.com/rejeep/7736123/raw | sh
  - export PATH="/home/travis/.cask/bin:$PATH"
  - export PATH="/home/travis/.evm/bin:$PATH"
  - evm install $EVM_EMACS --use
  - cask
env:
  - EVM_EMACS=emacs-23.4-bin
  - EVM_EMACS=emacs-24.1-bin
  - EVM_EMACS=emacs-24.2-bin
  - EVM_EMACS=emacs-24.3-bin
script:
  - emacs --version
  - make test
{% endhighlight %}

Add that content to a file called `.travis.yml` in your project root
and activate that project on the Travis website. Next time you push to
your repo, Travis will run and make sure your tests pass.

## Mocking/Stubbing

I recommend using mocks and stubs only when you have to. If you test a
function using mocks and stubs, the test will depend on the
implementation of the function. That means, if you refactor the
function, it is likely that you have to update the test as well. If
you write your test without mocks and stubs, you should be able to
change the test implementation without updating your test.

But, sometimes we need to use stubs and mocks. There are two libraries
in Emacs for this as far as I know:
[el-mock](https://github.com/rejeep/el-mock.el) and
[mocker](https://github.com/sigma/mocker.el). Since I am the
maintainer of el-mock, that is the library I will explain here.

All mocking and stubbing is done in a block called `with-mock`.

{% highlight scheme %}
(with-mock
 ;; ...
 )
{% endhighlight %}

El-mock has three functions that I primarily use in my tests: `stub`,
`mock` and `not-called`.

### stub

The stub function is used when you want to change the return value of
a function. By default, `nil` is returned. A use case for this is when
a function prints a message and you don't want that in your test
output.

{% highlight scheme %}
(with-mock
 (stub message)
 (my-funky-func))
{% endhighlight %}

You can also make a function return a specific value:

{% highlight scheme %}
(with-mock
 (stub f-exists? => t)
 (my-funky-func))
{% endhighlight %}

### mock

A mock is like a stub, only a bit more advanced. You can assert a
function to be called with specific argument (if `f-exists?` is not
called with `"/path/to/foo"`, the test will fail).

{% highlight scheme %}
(with-mock
 (mock (f-exists? "/path/to/foo"))
 (my-funky-func))
{% endhighlight %}

You can also specify how many times the function must be called:

{% highlight scheme %}
(with-mock
 (mock (f-exists? "/path/to/foo") :times 3)
 (my-funky-func))
{% endhighlight %}

If you want the mocked function to return a value, you can do that
with:

{% highlight scheme %}
(with-mock
 (mock (f-exists? "/path/to/foo") => t)
 (my-funky-func))
{% endhighlight %}

### not-called

This function is as simple as it sounds. It makes sure that a function
is not called.

{% highlight scheme %}
(with-mock
 (not-called f-delete)
 (my-funky-func))
{% endhighlight %}

There are a few other functions in el-mock, but I let you explore
those yourself!

## EVM

[EVM](http://github.com/rejeep/evm) (Emacs Version Manager) is a tool
that allow you to install multiple different versions of
Emacs. Functionality differ between Emacs version, even minor, so you
have to make sure that your packages work on a few different versions.

If you have installed EVM and a few Emacs versions, you can run the
tests in multiple versions using a simple script like this:

{% highlight bash %}
for version in 24.1 24.2 24.3; do
  EMACS=$(evm bin emacs-$version-bin) make test
done
{% endhighlight %}

If you want to make it even more fancy, check out `make -j`, which can
do the same thing, but simultaneously.

## Ert Async

To be able to test a project of mine called
[Prodigy](https://github.com/rejeep/prodigy.el), I had to work with
asynchronous functions, but Ert has no support for this, so I wrote a
wrapper library around Ert that does this. The library is called
[ert-async](https://github.com/rejeep/ert-async.el).

It works almost exactly like Ert. There is a function called
`ert-deftest-async` that works like `ert-deftest`, except that the
function argument list is used for done callbacks.

Let's say you have a function and you want to test that it callbacks
when done.

{% highlight lisp %}
(ert-deftest-async my-funky-func-test/callback (done)
  (my-funky-func (lambda () (funcall done))))
{% endhighlight %}

If the `done` function is called (without arguments and once) within
`ert-async-timeout` (default 4 seconds), the test will pass.

If the `done` callback is called more than once, the tests fails. This
can be useful at times. For example, if `my-funky-func` would have
callbacked more than once in the previous example, the test would have
failed, which we expect.

If the `done` function is called with an argument, the test also
fails. This can also be useful. Let's say that `my-funky-func` takes
two callbacks, one if the function was successful and one if it was
not. You can then call `done` with an argument in the error callback
to make sure that is never called.

{% highlight lisp %}
(ert-deftest-async my-funky-func-test/callback-successful (done)
  (my-funky-func
   (lambda ()
     (funcall done))
   (lambda ()
     (funcall done "should have been successful, but was not"))))
{% endhighlight %}

## Cask

[Cask](http://cask.github.io/) is key to everything I do when I
develop Emacs packages. I've used Cask in the previous posts, so you
should be fairly familiar with it.

I do however want to talk a bit about the future of Cask. As we speak
(or as you read), we are actively working on making Cask more than
just a dependency manager, which it is today. We are working on making
Cask a project management tool. That means that Cask will handle the
whole development cycle: setup, testing, development, packaging,
releasing and more.

I recommend you start adapting Cask right now, so that when this
functionality is ready, you are too.

That's it! I hope you found this post useful. Please post your
questions and comments below.
