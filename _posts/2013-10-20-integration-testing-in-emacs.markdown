---
title: Integration testing in Emacs
layout: post
categories:
 - emacs
 - testing
 - cask
 - ecukes
---

Sometimes, it's not a specific function you need to test, but a
complete use case. Unit tests are too low level and can only test
specific functions. Enter integration tests!

A great advantage with integration tests is that they are not
dependent on the underlying implementation. They test behaviour, not
code. That means you can refactor any code without changing a single
test. And when you are done, just run the tests again and make sure
nothing is broken.

That said, I am not saying that unit tests are useless. Unit tests are
more suited for library packages, rather than interactive. Interactive
packages may also have unit tests, but they should only test specific
functionality that are hard to cover in the integration tests.

In this post [Ecukes](https://github.com/ecukes/ecukes) is used, which
is an integration testing library for Emacs that lets you write
[Cucumber](http://cukes.info)-like tests for Emacs.

Add `ecukes` to the `Cask`-file to install.

{% highlight scheme %}
(source melpa)

(development
 (depends-on "ecukes"))
{% endhighlight %}

Run `cask` to install dependencies. Once installed, you should be able
to run the `ecukes` command:

{% highlight bash %}
$ cask exec ecukes -h
{% endhighlight %}

To setup testing for the current project, run:

{% highlight bash %}
$ cask exec ecukes new
{% endhighlight %}

This will create a directory called `features` with the following content:

* `features/this-package-name.feature` - The file where the tests (or
  features) are defined. Any file ending with `.feature` is a test
  file.
* `features/step-definitions/this-package-name-steps.el` - Each step
  in the `.feature`-files, need a corresponding step
  definition. Project specific step definitions goes in this file. Any
  file ending with `-steps.el` in this directory will be loaded as a
  step definition file. The package
  [Espuds](https://github.com/ecukes/espuds) contains commonly used
  ones.
* `features/support/env.el` - In this file the test environment is
  configured. Test packages are required, package state is reset
  between test runs, etc.

To run the test suite, run:

{% highlight bash %}
$ cask exec ecukes
{% endhighlight %}

There are tons of features in Ecukes. See the
[README](https://github.com/ecukes/ecukes) for more information.

### Example

In [my Emacs configuration](https://github.com/rejeep/emacs) I have a
function called `duplicate-current-line-or-region`. The function
duplicates the current line or region (if any). With prefix arg, the
line or region is duplicated that many times. This is a perfect use
case for an integration test, so let's write these Ecukes tests:

* Duplicate line
* Duplicate line multiple times
* Duplicate region
* Duplicate region multiple times

Create a new directory called `dup`.

{% highlight bash %}
$ mkdir dup
{% endhighlight %}

In the directory `dup`, create a `Cask`-file.

{% highlight bash %}
$ cask init --dev
{% endhighlight %}

With this content:

{% highlight scheme %}
(source melpa)

(package "dup" "0.0.1" "Duplicate stuff")

(development
 (depends-on "f")
 (depends-on "ecukes"))
{% endhighlight %}

Run `cask` to install all dependencies.

Initialize Ecukes:

{% highlight bash %}
$ cask exec ecukes new
{% endhighlight %}

Add this content to the file `features/dup.feature`:

{% highlight gherkin %}
Feature: Duplicate stuff

  Background:
    Given I switch to buffer "*dup*"
    And I clear the buffer
    And I insert:
      """
      Line 1
      Line 2
      """
    And I go to the beginning of the buffer
    And I bind key "C-c d" to "duplicate-current-line-or-region"

  Scenario: Duplicate line
    When I press "C-c d"
    Then I should see:
      """
      Line 1
      Line 1
      Line 2
      """

  Scenario: Duplicate line multiple times
    When I press "M-3 C-c d"
    Then I should see:
      """
      Line 1
      Line 1
      Line 1
      Line 1
      Line 2
      """

  Scenario: Duplicate region
    When I start an action chain
    And I press "C-SPC"
    And I press "C-n"
    And I press "C-c d"
    And I execute the action chain
    Then I should see:
      """
      Line 1
      Line 2
      Line 1
      Line 2
      """

  Scenario: Duplicate region multiple times
    When I start an action chain
    And I press "C-SPC"
    And I press "C-n"
    And I press "M-3 C-c d"
    And I execute the action chain
    Then I should see:
      """
      Line 1
      Line 2
      Line 1
      Line 2
      Line 1
      Line 2
      Line 1
      Line 2
      """
{% endhighlight %}

Add this content to the file `features/support/env.el`:

{% highlight scheme %}
(require 'f)

(defvar dup-support-path
  (f-dirname (f-this-file)))

(defvar dup-features-path
  (f-parent dup-support-path))

(defvar dup-root-path
  (f-parent dup-features-path))

(add-to-list 'load-path dup-root-path)

(require 'dup)
(require 'espuds)
(require 'ert)
{% endhighlight %}

Add this content to the file `features/step-definitions/dup-steps.el`:

{% highlight scheme %}
(Given "^I bind key \"\\([^\"]+\\)\" to \"\\([^\"]+\\)\"$"
  (lambda (key fn-name)
    (global-set-key (kbd key) (intern fn-name))))

(When "^I go to the beginning of the buffer$"
  (lambda ()
    (call-interactively 'beginning-of-buffer)))
{% endhighlight %}

And add the implementation to `dup.el`:

{% highlight scheme %}
(defun duplicate-current-line-or-region (arg)
  "Duplicates the current line or region ARG times.
If there's no region, the current line will be duplicated. However, if
there's a region, all lines that region covers will be duplicated."
  (interactive "p")
  (let (beg end (origin (point)))
    (if (and (region-active-p) (> (point) (mark)))
        (exchange-point-and-mark))
    (setq beg (line-beginning-position))
    (if (region-active-p)
        (exchange-point-and-mark))
    (setq end (line-end-position))
    (let ((region (buffer-substring-no-properties beg end)))
      (dotimes (i arg)
        (goto-char end)
        (newline)
        (insert region)
        (setq end (point)))
      (goto-char (+ origin (* (length region) arg) arg)))))

(provide 'dup)
{% endhighlight %}

Now run the tests with:

{% highlight bash %}
$ cask exec ecukes --no-win --quiet
{% endhighlight %}

*(We need to provide the `--no-win` option because Emacs batch mode does
not handle lines)*

How easy was that!? The tests not only makes sure that the code works,
it also documents it.

Here are a few packages using Ecukes that you can use to find inspiration:

* [https://github.com/cask/cask](https://github.com/cask/cask)
* [https://github.com/magnars/expand-region.el](https://github.com/magnars/expand-region.el)
* [https://github.com/Fuco1/smartparens](https://github.com/Fuco1/smartparens)

That's it on integration testing in Emacs. I plan to write one last
post on the _"testing in Emacs"_ subject, which will be about things
related to testing, such as running tests on Travis, using stubs and
mocks, etc.

Feel free to post any question below!
