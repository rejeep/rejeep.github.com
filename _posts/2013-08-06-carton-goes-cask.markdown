---
title: Carton goes Cask
layout: post
categories:
 - emacs
 - carton
 - cask
---

Because of a name clash with another project called Carton (see
<https://github.com/rejeep/cask.el/issues/20>), the project have
changed name to Cask.

With this new release, there are a lot of new awesome features (> 150
commits), but let's get rid of the boring stuff first, migration.

## Migration

Migration is simple and should just take a minute or two.

1. Use the `cask` command instead of the `carton` command
2. If your Emacs configuration dependes on `carton`, depend on `cask` instead:

   `(depends-on "carton")` => `(depends-on "cask")`

3. Rename the file `Carton` to `Cask`
4. Rename the installation directory (default `~/.carton`) to `~/.cask` (don't forget to update the path in you shell config)

_(And ohh, don't forget to update your `.travis.yml` and `.gitignore` files)_

## Source aliases

Who remember the URL to a package archive? I never do. So instead of:

    (source "melpa" "http://melpa.milkbox.net/packages/")

use the alias:

    (source melpa)

The README contains a list of all pre defined sources: <https://github.com/rejeep/cask.el/#source>

## Project bin added to PATH

Some projects have a binary command, for example
[Ecukes](https://github.com/rejeep/ecukes),
[Ert runner](https://github.com/rejeep/ert-runner) and this Cask of
course. To avoid writing a `Makefile` like this:

    ECUKES = $(shell find elpa/ecukes-*/ecukes | tail -1)

    test:
        cask exec ${ECUKES} features

    .PHONY: test

All projects that have a directory called `bin` are added to `PATH`
when running `cask exec`. So the `Makefile` now becomes:

    test:
        cask exec ecukes features

    .PHONY: test


## EPL

Until this point, Cask supported Emacs 23 and Emacs 24. Now we also
support Emacs snapshot and we do it using
[@lunaryorn](https://github.com/lunaryorn)'s excellent EPL package
(which at the moment is bundled in the Cask source). If you ever want
to use some function in `package.el`, do it via `EPL`.

## One elpa directory per version

Previously, all project dependencies were located in a directory
called `elpa` in the project root. This directory has changed name to
`.cask`.

That's not it. Now each version has its own sub directory in
`.cask`. Here's an example from [Ecukes](http://ecukes.info):

    $ tree -d .cask
    .cask
    |-- 23.4.1
    |   |-- elpa
    |       |-- ansi-20121008.1920
    |       |-- archives
    |       |   |-- melpa
    |       |-- dash-20130712.2307
    |       |-- el-mock-20121004.2057
    |       |-- s-20130617.1851
    |-- 24.3.1
        |-- elpa
            |-- ansi-20121008.1920
            |-- archives
            |   |-- melpa
            |-- dash-20130712.2307
            |-- el-mock-20121004.2057
            |-- s-20130617.1851

## Cask binary rewritten in Emacs Lisp

To avoid the pain of BASH and make it easier to add support for more
platforms, the `cask` binary have been implemented in Emacs Lisp.

Next up is release `v0.5`. Check out what's included:
<https://github.com/rejeep/cask.el/issues?milestone=2&state=open>
