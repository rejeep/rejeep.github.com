---
title: JDE 2.3.6 Portage ebuild
layout: post
categories:
 - ebuild
 - emacs
 - gentoo
 - java
 - portage
---

If you're a Java programmer and use Emacs you most likely use
[JDE](http://jdee.sourceforge.net). Development has been on hold for
quite a while now, but the lead developer Paul Kinnucan has recently
been doing some updates.

This is a quote from the
[Emacs Wiki](http://www.emacswiki.org/emacs-en/JavaDevelopmentEnvironment)

<pre>January 2009: latest version 2.3.5.1 is very old and has lots of known bugs.
Don’t use it and use the unreleased 2.3.6 instead, available as a branch
inside the Subversion repositoy at:
https://jdee.svn.sourceforge.net/svnroot/jdee. The main Subversion branch
(labelled as JDEE 2.3.5.2) has less fixes than 2.3.6. So use 2.3.6! (unless new
versions had appeared by the time you read this).</pre>

For you Gentoo (Portage) users, I made an ebuild for this
release. It's available among my ebuilds at
[Github](http://github.com/rejeep/ebuilds). The ebuild also include a
patch for removing unused imports.

Note that you need Ant and SVN to use this ebuild!
