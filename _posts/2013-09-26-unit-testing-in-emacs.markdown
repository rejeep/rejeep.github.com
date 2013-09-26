---
title: Unit testing in Emacs
layout: post
categories:
 - emacs
 - testing
 - cask
 - ert-runner
---

All right, let's start out with some basic unit testing. Unit testing
is about testing the behavior of a specific function. A good rule of
thumb is that if an Elisp function is
[interactive](http://www.gnu.org/software/emacs/manual/html_node/elisp/Using-Interactive.html),
it should most likely *not* be tested with a unit test, but rather
with an integration test. The reason is that the unit test will not
use the function as it is supposed, via `M-x` or a key binding, but
with an explicit function call. The unit test can use
`call-interactively` to simulate similar behavior, but again, it's
better to test the function *exactly* as it is intended to be used,
using integration tests.

In Emacs 24, there is a built in testing library called
[Ert (Emacs Regression Testing)](http://www.gnu.org/software/emacs/manual/html_mono/ert.html).
There are a coupple of others, but this post will only use Ert. To
create a test, use `ert-deftest`.

{% highlight scheme %}
(ert-deftest my-test ()
 ;; ...
 )
{% endhighlight %}

Ert also includes the assertion functions: `should`, `should-not` and `should-error`:

{% highlight scheme %}
(ert-deftest my-test ()
 (should (equal (my-fn) expected))
 (should-not (equal (my-other-fn) expected))
 (should-error (error "BOOM")))
{% endhighlight %}

Because the power of macros in Emacs, it is possible to use any
expression in the assertion functions, and at failure Ert will print
the expanded and evaluated tree. In other languages you would have to
use helpers, such as `should-equal`, `should-not-equal` and
`should-contain` to get a decent failure report.

### Ert runner

At the moment, unit tested Emacs packages all work very different. It
is very annoying that for each package you want to contribute to,
have to figure out how testing works. So I created a package called
[ert-runner](https://github.com/rejeep/ert-runner.el) in hope that
unit tests for Emacs packages would work the same way.

To install, add `ert-runner` to the package `Cask`-file.

{% highlight scheme %}
(source melpa)

;; ...

(development
 (depends-on "ert-runner"))
{% endhighlight %}

Once installed, you should be able to run the `ert-runner` command. To
setup testing, run:

{% highlight bash %}
$ cask exec ert-runner init
{% endhighlight %}

This will create a directory called `test` with the following content:

* `test/test-helper.el` - This file is included in each test run
  before any test file is loaded. This is a good place for common test
  helper functions.
* `test/this-package-name-test.el` - A test file. All files in the
  `test` directory ending with `-test.el` is considered a test file.

To run all test files, run:

{% highlight bash %}
$ cask exec ert-runner
{% endhighlight %}

See usage information for more information:

{% highlight bash %}
$ cask exec ert-runner -h
{% endhighlight %}

### Example

To give you a better understanding about how this works in practice,
I will create a simple package that will be unit tested. The package
has one simple purpose and that is to find the project root given a
file or directory. The definition of a project root is that there
exists a file or directory in that directory with a name included in a
list (that is defined in the package).

If you want to follow along this example locally, start of by
installing [Cask](https://github.com/cask/cask) if you haven't
already.

The function that finds the root is called `root-find` and will be
used like this:

{% highlight scheme %}
(root-find)
(root-find "/path/to/dir")
(root-find "/path/to/file.txt")
{% endhighlight %}

Start off by creating a directory called `root`:

{% highlight bash %}
$ mkdir root
{% endhighlight %}

In the `root` directory, create a `Cask`-file with this content:

{% highlight scheme %}
(source melpa)

(package "root" "0.0.1" "Find project root.")

(development
 (depends-on "f")
 (depends-on "ert-runner"))
{% endhighlight %}

Run `cask` to install.

Create the ert-runner test files:

{% highlight bash %}
$ cask exec ert-runner init
{% endhighlight %}

The function will be working with files and directories, so the tests
can either use a helper function that run in a sandbox environment or
use stubs and mocks. If stubs and mocks can be avoided, it's always
the better option. So I will to use a sandbox environment for a few
reasons:

* Stubs and mocks fakes behavior, while the sandbox environment will
  do the testing exactly like the function will be used.
* Stubbing and mocking recursive functions in Emacs is not easy given
  the provided tools.
* Creating a sandbox environment is easy.

The file `test/root-helper.el` defines the sandbox helper function and
require the package:

{% highlight scheme %}
(require 'f)

(defvar root-test-path
  (f-dirname (f-this-file)))

(defvar root-code-path
  (f-parent root-test-path))

(defvar root-sandbox-path
  (f-expand "sandbox" root-test-path))

(require 'root (f-expand "root.el" root-code-path))

(defmacro with-sandbox (&rest body)
  "Evaluate BODY in an empty temporary directory."
  `(let ((default-directory root-sandbox-path))
     (when (f-dir? root-sandbox-path)
       (f-delete root-sandbox-path :force))
     (f-mkdir root-sandbox-path)
     ,@body))
{% endhighlight %}

Given the definition of `root-find`, these are the test cases I
want. The function is called with:

* no argument (use `default-directory`)
* a directory as argument
* a file as argument
* a directory, which is the project root
* a directory that is not within a project directory

Here are the corresponding Ert-tests:

{% highlight scheme %}
(ert-deftest root-test/no-argument ()
  "Should use `default-directory' when no argument."
  (with-sandbox
   (f-mkdir ".git")
   (should (equal (root-find) root-sandbox-path))))

(ert-deftest root-test/directory-as-argument ()
  "Should find root directory when directory as argument."
  (with-sandbox
   (f-mkdir ".git")
   (f-mkdir "foo" "bar" "baz")
   (should (equal (root-find "foo/bar/baz") root-sandbox-path))))

(ert-deftest root-test/file-as-argument ()
  "Should find root directory when file as argument."
  (with-sandbox
   (f-mkdir ".git")
   (f-mkdir "foo" "bar" "baz")
   (f-touch "foo/bar/baz/qux.txt")
   (should (equal (root-find "foo/bar/baz/qux.txt") root-sandbox-path))))

(ert-deftest root-test/project-root-as-argument ()
  "Should find root when root as argument."
  (with-sandbox
   (f-mkdir ".git")
   (should (equal (root-find root-sandbox-path) root-sandbox-path))))

(ert-deftest root-test/no-project-root ()
  "Should return nil when no root."
  ;; Obviously not the best test since /bin may not exist, but should
  ;; work in most cases.
  (with-sandbox (should-not (root-find "/bin"))))
{% endhighlight %}

Now that the tests are written, let's implement the function. As a
good exercise, try writing the function yourself. As always, write the
simplest possible code, just so that it works. It does not have to be
pretty, as long as the tests pass. This is my version:

{% highlight scheme %}
(defvar root-files '(".git" ".projectile" ".cask")
  "List of names that defines a project root.")

(defun root-find (&optional dir)
  "Find project root, starting at DIR."
  (setq dir (or dir default-directory))
  (if (file-regular-p dir)
      (setq dir (expand-file-name ".." dir)))
  (let (is-root-dir (parent (expand-file-name ".." dir)))
    (unless (equal (file-truename parent) (file-truename dir))
      (dolist (root-file root-files)
        (when (file-exists-p (expand-file-name root-file dir))
          (setq is-root-dir t)))
      (if is-root-dir dir (root-find parent)))))

(provide 'root)
{% endhighlight %}

Now that there is a working version, let's put some sugar on
it. Change the `Cask`-file to include
[f](https://github.com/rejeep/f.el) and
[dash](https://github.com/magnars/dash.el):

{% highlight scheme %}
(source melpa)

(package "root" "0.0.1" "Find project root.")

(depends-on "f")
(depends-on "dash")

(development
 (depends-on "ert-runner"))
{% endhighlight %}

Using those packages, the refactored function looks like this:

{% highlight scheme %}
(defun root-find (&optional dir)
  "Find project root, starting at DIR."
  (let ((root
         (f-up
          (lambda (path)
            (--any? (f-exists? (f-expand it path)) root-files))
          (or dir default-directory))))
    (unless (f-root? root) root)))
{% endhighlight %}

And I can be sure everything still works, because the tests pass. Yay!

---

If you want to get some inspiration from Emacs packages that have a
good unit test suite, check out these:

* [https://github.com/rejeep/f.el](https://github.com/rejeep/f.el)
* [https://github.com/flycheck/flycheck](https://github.com/flycheck/flycheck)
* [https://github.com/rejeep/commander.el](https://github.com/rejeep/commander.el)
* [https://github.com/rejeep/ansi.el](https://github.com/rejeep/ansi.el)

That's it for this post folks, hopefully you learned something. Next
up is integration testing!
