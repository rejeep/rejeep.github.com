---
title: Ecukes - Cucumber for Emacs
layout: post
categories:
 - emacs
 - elisp
 - cucumber
 - testing
---

I just finished the first release of a project of mine called
[Ecukes](http://ecukes.info). Ecukes is a package for Emacs that makes
it possible to write Cucumber like tests for Emacs packages.

This release includes the most basic parts of Cucumber and I would
consider it usable, but still under development.

To give you an example of what you can do with Ecukes, check out this example:

{% highlight gherkin %}
Feature: Work with text
  In order to work with text
  As an Emacs user
  I want to do freaky stuff with it
  
  Background:
    Given I am in the buffer "*Freaky Stuff*"
    And the buffer is empty

  Scenario: Transpose words
    When I insert "word1 word2"
    And I go to point "6"
    And I press "M-t"
    Then I should see "word2 word1"

  # ...
{% endhighlight %}

Each step needs to be translated with a step definition. These are the
steps for the feature above. All these steps are however already in
[Espuds](http://github.com/rejeep/espuds), which is a collection of
commonly used step definitions.
{% highlight scheme %}
(require 'edmacro)

(When "^I press \"\\(.+\\)\"$"
      (lambda (keybinding)
        (execute-kbd-macro (edmacro-parse-keys keybinding))))

(Given "I am in the buffer \"\\(.+\\)\""
       (lambda (buffer)
         (switch-to-buffer (get-buffer-create buffer))))

(And "the buffer is empty"
     (lambda ()
       (erase-buffer)))

(When "I insert \"\\(.+\\)\""
      (lambda (contents)
        (insert contents)))

(And "I go to point \"\\(.+\\)\""
     (lambda (point)
       (goto-char (string-to-number point))))

(Then "I should see \"\\(.+\\)\""
      (lambda (should-see)
        (let ((actual (buffer-substring-no-properties (point-min) (point-max)))
              (expected should-see))
          (assert
           (search expected actual) nil
           (concat "Expected \"" actual "\" to include \"" expected "\"")))))
{% endhighlight %}

This is how running the feature looks like:
<img src="/images/ecukes.png" class="screenshot" />

Check out [Ecukes website](http://ecukes.info) and the
[source code](http://github.com/rejeep/ecukes).

You might also want to check out two Emacs packages of mine that are tested with Ecukes:

* Drag Stuff - <http://github.com/rejeep/drag-stuff>
* Wrap Region - <http://github.com/rejeep/wrap-region>
