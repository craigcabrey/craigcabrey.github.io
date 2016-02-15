---
layout: post
title: "HFOSS: Weekly Update"
description: "Weekly update for the RIT HFOSS course."
tags:
  - hfoss
---

There is not much to report this week that directly relates to what is going on
in the class. However, due to recent issues with GitHub[^1][^2], I have been
investigating alternatives for code hosting platforms.

There are a few options available off the top of my head:

1. [Bitbucket](https://bitbucket.org/)
1. [GitLab (hosted via GitLab.com)](https://about.gitlab.com/gitlab-com/)
1. [GitLab (self-hosted)](http://gitlab.org/)
1. [Phabricator](http://phabricator.org/)
1. [Gogs](https://gogs.io/)
1. [SourceForge](https://sourceforge.net/)

There are two services that I will immediately disregard: Bitbucket and
SourceForge. Bitbucket appears to be a nice service (and allowed unlimited
private repositories), but is a proprietary service. Thus, it wouldn't be much
better than GitHub for Free software hosting. Secondly, SourceForge has had a
number of controversies[^3] recently that leads me to avoid it altogether.

GitLab is essentially a GitHub clone and is probably the most well known
alternative at this point. But my goal was to find something with even more
functionality than GitHub if I'm going to go through with self-hosting my own
solution.

I decided to give Phabricator a try. Originally developed internally at
Facebook, Phabricator has since been spun off into its own company that
sponsors the development of the software. One of the alluring features of
Phabricator is that it is actually a suite of tools. It offers project
management, task management, code review, repository management, and more. Its
aim is to integrate the entire software development workflow into one place.

The setup process was straightforward (although that statement highly depends
on your experience deploying and administering complex applications). The
technology of Phabricator is a traditional LAMP stack (Linux, Apache, MySQL,
PHP); although I swapped out Apache for nginx and MySQL for MariaDB.

So far, I'm fairly happy with the results. For projects that I don't want to
public while in the development phase, it should work nicely. However, for
everything else I'll definitely continue to use GitHub as a public hosting
platform. Beyond its fantastic (and fast) code hosting, GitHub has a social
aspect to it that as of now cannot be matched anywhere else (known as the
"networking" effect).

---

[^1]: [January 28th Incident](https://github.com/blog/2106-january-28th-incident-report)

[^2]: [GitHub: The Full Inside Story](http://www.businessinsider.com/github-the-full-inside-story-2016-2)

[^3]: [Malware on SourceForge](http://www.howtogeek.com/218764/warning-don%E2%80%99t-download-software-from-sourceforge-if-you-can-help-it/)
