---
title: Mactag with FSSM
layout: post
categories:
 - emacs
 - rails
---

When I first released Mactag I got a question about automatic updating
of TAGS-files when changing file contents. Today Mactag can do this,
with a little bit of help.

First, you need Mactag version **0.4.0** and the **fssm** Rubygem. The
new version of Mactag includes a rake task called
**mactag:server**. Calling this will start a server that listens for
file changes in the project.

With the server started, changes to project files, creation of new
files and removal of files will trigger Mactag to update, create or
remove the corresponding TAGS-file.

Your editor however needs to be aware of TAGS-files changes. I created
an Emacs script called [Mactag.el](http://github.com/rejeep/mactag.el)
that makes sure **find-tag** have the correct TAGS-tables before
lookup.
