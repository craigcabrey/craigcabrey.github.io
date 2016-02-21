---
layout: post
title: "The Case of the Missing Apps"
description: "A brief summary on tracking down and fixing a bug in GTK+."
tags:
    - hfoss
---

About a week ago, a friend of mine received his shiny new ThinkPad X220. He was
looking for an upgrade from his aging Dell, but didn't want something that
broke the bank. He was also looking to move away from Ubuntu, so I suggested
Fedora as the next logical step.

As I was showing him around GNOME Shell, he asked for some functionality that
wasn't baked in by default. I pointed him towards extensions and showed him the
[Auto Move Windows](https://extensions.gnome.org/extension/16/auto-move-windows/)
one in particular. He wanted to automatically pin the Terminal to a specific
workspace.

However, there was a problem. Terminal, along with a whole host of other apps,
weren't listed.

{: .center}
![The offending GtkAppChooserWidget part of the extension preferences.](/assets/article_images/2016-02-21-the-case-of-the-missing-apps/GtkAppChooserWidget.png "GTKAppChooserWidget")

### Uh Oh

Not wanting to leave him with a bad first impression, I immediately started
looking to see if others were having the same problem.

Based on comments left at the extension page, we weren't the only ones having
this problem. But no fix was in sight, so I decided to give it a stab myself.

## Bug Hunting

The first step in the process was determining what part of the stack
contained the bug.

To do so, I started with writing my own bit of JavaScript code that
reconstructed a similar application selection dialog. Specifically, I ended
up narrowing it down to the `GtkAppChooserWidget`:

<script src="https://gist.github.com/craigcabrey/cea560f4823f69211bd8.js">
</script>

Running this bit of code exhibits the same problem as the extension. I couldn't
just immediately assume the problem was in the JavaScript bindings. In order to
rule out the language bindings being at fault, I wrong a similar test in
Python:

<script src="https://gist.github.com/craigcabrey/faf955ff7bc278e557d5.js">
</script>

Same issue. At this point, I was convinced the problem was in GTK+ itself. I
found the source for `GtkAppChooserWidget` and started looking.

That led me
[here](https://github.com/GNOME/gtk/blob/master/gtk/gtkappchooserwidget.c#L786),
which looked like a candidate ripe for investigation. I had a bit of trouble
finding the implementation, so I popped onto `#gtk+` for some help. They
pointed me in the direction of the glib repository, where I finally found it
living
[here](https://github.com/GNOME/glib/blob/cd1eba043c90da3aee8f5cd51b205b2e2c16f08e/gio/gdesktopappinfo.c#L4251).

After manually inspecting `g_app_info_get_all ()`, I did the same thing as my
prior two attempts, this time using C to call the function directly:

<script src="https://gist.github.com/craigcabrey/b4c4b1c7cb1607489f0d.js">
</script>

No luck. All the output looked correct. Back to inspecting GTK+ again. Finally,
I stumbled across a [suspicious looking condition](https://github.com/GNOME/gtk/blob/master/gtk/gtkappchooserwidget.c#L558-L560):

~~~
if (!g_app_info_supports_uris (app) && !g_app_info_supports_files (app))
    continue;
~~~
{: .language-c}

Essentially, if an application doesn't register a special URI or file type, it
isn't listed. Well that can't be right.

Sure enough, removing this small condition restores the correct functionality.

## The Fix

After chatting with some of the devs on `#gtk+`, I was directed to submit a
report on GNOME's Bugzilla. It was designated
[bug 762410](https://bugzilla.gnome.org/show_bug.cgi?id=762410) and I have
attached a proposed patch to fix the issue.

Seeing as GTK+ is a large and complex system, I don't expect it to be
immediately accepted. There will probably be some amount of back and forth
before a final fix can be decided upon.
