---
title: Introducing citrus-mode
layout: post
categories:
 - emacs
 - elisp
 - citrus
---

[Citrus](http://mjijackson.com/citrus/) is a library for Parsing
Expressions in Ruby.

This is a simple grammar in Citrus for addition. As you can see, the
grammar language very simple:

    grammar Addition
      rule additive
        number plus (additive | number)
      end

      rule number
        [0-9]+ space
      end

      rule plus
        '+' space
      end

      rule space
        [ \t]*
      end
    end

The way to use the grammar is:

    require 'citrus'

    Citrus.load 'addition'

    m = Addition.parse '1 + 2 + 3'
    m.value # 6


I wrote [citrus-mode](https://github.com/rejeep/citrus-mode), which is a major mode for Emacs to edit Citrus
files.
