---
title: Ecukes version 0.0.2
layout: post
categories:
 - emacs
 - elisp
 - testing
 - ecukes
---

Version 0.0.2 of [Ecukes](http://ecukes.info) is out. The release
mostly contains bug fixes and better testing.

Here's a list of changes:

* The message function is not adviced anymore. Instead the \*Messages\* buffer is checked for messages.
* Added helper functions to avoid cluttering down the test output. 
* Added Setup and Teardown hooks. Setup is run once before and Teardown once after.
* Table steps argument is now a list, where the car is the header and the cdr are the rows.
* Creating a new project is done with "ecukes --init" instead of "ecukes-init" (Thanks Daniel Hackney).

Some updates has also been made in [Espuds](http://github.com/rejeep/espuds).

Since the first release of Ecukes
[Drag Stuff](http://github.com/rejeep/drag-stuff)
and
[Wrap Region](http://github.com/rejeep/wrap-region)
have implemented tests with Ecukes. Work is also being done to implement tests for
[Keats](http://github.com/rejeep/keats/tree/facelift)
and
[Rinari](http://github.com/rejeep/rinari/tree/testing).
