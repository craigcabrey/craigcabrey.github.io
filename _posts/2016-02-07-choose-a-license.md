---
layout: post
title: "Choose a License"
description: "A brief rationale on the importance of choosing a license."
tags:
    - hfoss
---

Licensing is one of the most important topics in the free software/open
source[^1] world and one that is unfortunately often overlooked or
misunderstood by a large percentage of the community.

When it comes to licenses, there are typically four different options:

1. No license (default copyright law applies)
1. Proprietary/commerical
1. Permissive
1. Copyleft

I want to focus on the last two items in that list and the differences between
them and the consequences of choosing one over another.

Generally, when someone is talking about a permissive license, they are usually
referring to a [BSD-style license](https://en.wikipedia.org/wiki/BSD_licenses).
Although this is not always the case, BSD-style licenses are undoubtedly the
most popular permissive licenses in use for open source software today.

On the flipside, the most common copyleft licenses in use are the [GNU General
Public License](http://www.gnu.org/licenses/licenses.en.html) family of
licenses (GPL, LGPL, AGPL, etc).

While there are many different licenses from which to choose, you should fully
read any license you choose to apply to your project. Doing so will ensure that
you will not be surprised later on when someone forks your project and invokes
a clause of which you may have been aware. GitHub hosts a
[site](http://choosealicense.com/) that guides you in choosing an appropriate
license based on the features you require.

## Permissive Licenses

For the past several years, permissive licenses have seen tremendous growth
(arguably alongside the rise of GitHub and "social coding"). At least part of
this growth can be attributed to the appeal of having a simple and easy to
understand license.

Permissive licenses are extremely simple and minimal licenses that only take a
few minutes to read. These licenses have only a few clauses that warn the user
of no warranty, allow derivative works of the code, and sometimes require
attribution where appropriate.

For example, one of the most popular permissive licenses is the
[MIT License](https://opensource.org/licenses/MIT). The following is the
primary clause that dictates how the licensed work may be used, modified,
distributed, and so on:

~~~
Permission is hereby granted, free of charge, to any person obtaining a copy of
this software and associated documentation files (the "Software"), to deal in
the Software without restriction, including without limitation the rights to
use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies
of the Software, and to permit persons to whom the Software is furnished to do
so, subject to the following conditions:
[...]
~~~

It is critical to understand that when this style of license is employed, the
work can be used in almost any fashion. If a company wants to incorporate it
into a commercial proprietary product without compensating you as an author,
they are well within their rights.

## Copyleft Licenses

Copyleft licenses emerged in the mid 1980s when Richard Stallman introduced the
[GNU Manifesto](https://en.wikipedia.org/wiki/GNU_Manifesto).

The primary goal of copyleft licenses is to ensure that copies and/or
modifications of the original work also remain copyleft. In effect, the
original work can never become proprietary; doing so would be a violation of
the license and United States copyright law.

In contrast to permissive licenses, copyleft licenses are not simple and easy
to read. In fact, one of the main criticisms many people have about copyleft
licenses is that they tend to be written in "legalese".

However, this is a necessary evil when you consider what these licenses are
attempting to accomplish. Rather than using copyright law for its original,
intended purpose, copyleft licenses pervert traditional copyright to work in
its favor. By taking advantage of the strength of copyright law (in the United
States, at least), copyleft licenses are able to dictate certain terms which
give them a viral nature (which incidentally, is why these licenses are also
sometimes referred to as viral licenses[^2]).

Indeed, a number of copyleft licenses have been tried in a court of law and
have been upheld. The most famous case is of
_ERIK ANDERSEN and ROB LANDLEY v. Monsoon Multimedia, Inc._, which you can read
more about [here](http://torquemag.io/2013/03/busybox/).

### Misconceptions

When talking with others about copyleft licenses, I have often found a number
of misconceptions that are rampant.

For example, there is a common belief that you cannot sell GPL'd software. This
is blatantly not true; section 1 of the GPLv2 contains this clause:

~~~
You may charge a fee for the physical act of transferring a copy, and you may
at your option offer warranty protection in exchange for a fee. 
~~~

The point is, you must discard any misconceptions you may have about copyleft
licenses until **you read the license itself**.

## Which is better?

> ## _Permissive licenses protect the rights of the developer, while copyleft licenses protect the rights of the user._

The most difficult part of adding a license to your project is, of course,
choosing which license to use. It all comes down to a matter of personal
preference and expectations.

If you expect derivatives to contribute back in any meaningful way, it makes
sense to use a copyleft license. If your primary goal is to make the use of
your project easy and accessible to other developers, it may make more sense to
use a permissive license.

In the end, the best way to summarize it is the following: permissive licenses
protect the rights of the developer, while copyleft licenses protect the rights
of the user.

No matter which side of the fence you fall on, it is crucial to choose a
license from the beginning for your project. Make your choice clear and visible
so that there is no confusion to users or potential contributors.

----

[^1]: For the purposes of this post, I consider the free software and open
      source communities to be one and the same. I'm aware that the FSF would
      not agree with this viewpoint.

[^2]: The term "viral license" is sometimes meant to be derogatory when
      referring to a copyleft license, but in this context it is not.
