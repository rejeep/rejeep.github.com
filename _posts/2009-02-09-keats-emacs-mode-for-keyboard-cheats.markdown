---
title: Keats: Emacs mode for Keybinding Cheats
layout: post
categories:
 - cheat
 - emacs
 - keybindings
 - keats
---

In Emacs, there's **a lot** of keybindings to remember. From time to
time it can be a quite large time gap between using different (usually
major) modes. Then I have no chance remembering all keybinding. **C-x b**
and **C-x m** helps, but to be able to put in my own description I usually
keep a cheat file that I manage in **org-mode** that looks something like
this:
<pre>| C-c C-v C-c   | Compiles this file |
| C-c C-v C-r   | Runs shis file     |
| C-x C-u       | Upcase region      |
| C-S-backspace | Kill whole line    |
...</pre>

This works, but (as most Emacs users) I don't like to leave Emacs and
if I can avoid it, don't even leave the current buffer. But in order
to look up a cheat I have to leave the current buffer or split screen
or something to get me back easy afterwards. This means that I have to
find the file or buffer for each time I want to find or add a cheat.

Anyway, since this is way to painful, I decided to write a mode to
handle keybinding cheats (or keats) for me. I didn't even bother
looking for a mode, but instead wrote my own for fun and
learning. This mode is actually two modes: **Keats** and **Keats Interactive.**

Keats is hosted at Github:
[http://github.com/rejeep/keats](http://github.com/rejeep/keats)

In the file comments, there's plenty of installation and usage
instructions. But here's some basic usage.

## Keats
This is a minor mode which allows you to manage keats without having
to leave the current buffer. These keybindings are available:

**Add**
Add a new: **C-c k a**

**Edit**
Edit an existing: **C-c k e**

**Remove**
Remove an existing: **C-c k r**

**Print description**
Print the description of an existing: **C-c k d**

**Search**
Search for a word in description: **C-c k s**

**Write**
Write to file: **C-c k w**. This is done automatically when Emacs is
killed and depending on **keats-save-at** variable, at add, edit and
remove (see comments in code).

**Interactive**
Interactive mode: **C-c k i**. Starts interactive mode (see below).

## Keats Interactive
Why two modes? Well Keats mode is good if you find out a new
keybinding when you are working that you fast want to save without it
disturbing your workflow. Keats Interactive mode is good to use if you
want to add, edit or remove many keats at the same time. To start this
mode there a binding for it in Keats mode: **C-c k i**. Starting
this mode will bring up a new buffer with all keats (search uses this
mode, and in this case only the keats that matched the query will be
shown). There's a couple of keybinding for this buffer:

**Add**
Add a new: **a**

**Edit**
Edit an existing: **e**

**Remove**
Remove an existing: **r**

**Write**
Write to file: **w**

**Search**
Search for a word in description: **s**

**Next**
Moves to the next in list: **n**

**Previous**
Moves to the previous in list: **p**

**Run/Execute**
Run command for which key at point is connected to: **RET**. Prefix key
for this command will kill the buffer before executing the command.

**Quit**
Quits the mode: **q**

Keybindings for add, edit, remove and description in both modes can be
used with a prefix key **C-u**. In this case there will be no reading of
the keybinding, but it will be entered in words. This is good to use if
a keybinding contains **C-g** or **RET**, which otherwise wouldn't be
possible to add. This is also good if you don't want to enter a key,
but a function name.

## Future and Feature
In future versions I plan to implement tags support. And one thing I
haven't yet decided, is if it should be possible to enter the same key
more than once. For example one might want to store the key **n** which
can mean different things in many modes. If you can think of any other
features, please let me know.
