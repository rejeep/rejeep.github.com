---
title: Emacs package development
layout: post
categories:
 - emacs
 - cask
 - ecukes
---

It sucks, at the moment..!

Historically, a single file was uploaded to the Emacs Wiki and that was it, no tests, no nothing. Folks would download the file and add to their Emacs configuration. With some recent tools, such as [MELPA](http://melpa.milkbox.net) and [Cask](https://github.com/cask/cask), package development has improved, but it's still not good. Why?

Did you experience the [dash.el](https://github.com/magnars/dash.el) breakage a while ago? Or the recent [Ecukes](https://github.com/rejeep/ecukes.el) breakage? These failures really suck, but with the current Emacs package development tools, it's impossible for Emacs package developers to deliver 100% quality.

When I broke Ecukes, I tested the changes locally on all my Ecukes projects and even on some other people's projects, and it worked great! I even tested with Ecukes byte compiled. Everthing was great, so I released it and ... **BOOM**:

    $ cd ~/Code/cask.el
    $ cask update
    $ make
     
    End of file during parsing: ~/Code/cask.el/.cask/24.3.1/elpa/ecukes-20130830.930/ecukes-core.elc

What the fuck?

It's not really relevant what was causing the issue other than that it was not possible to detect unless installed the exact same way as is done when using for real.

It's really a guessing game. It looks good locally. Great, I push to master and wait five hours for MELPA to build. If something is wrong, I debug and then push a fix and wait another five hours for MELPA... Fingers crossed, I really hope I fixed it.

So how do we fix this? Because it needs to be fixed! I really don't want to break other people's packages and configurations and I don't want other people breaking mine. The solution is [Cask](https://github.com/cask/cask). Me and [@lunaryorn](https://github.com/lunaryorn) made the decission to extend Cask from just being a dependency manager to being a project management tool. That means Cask will handle the whole development cycle; development, dependencies, testing, packaging, releasing, etc... Kind of like [Leiningen](https://github.com/technomancy/leiningen) for Clojure.

Cask will not only run the tests on the `.el` files. It will also package the project, exactly like done in production and run the tests on the built package. That means what you publish to MELPA is what you tested locally.

There are a lot of other functionality that will be added to help make Emacs package development great. Check out the `0.6` milestone <https://github.com/cask/cask/issues?milestone=3&state=open> for our plans.

We would love to hear what you would like to see, so get in touch!
