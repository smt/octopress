---
title: OOCSS, For Great Justice
date: 2011-07-17 13:55
layout: post
categories: [css]
---

In my [previous post](/blog/2011/07/09/responsible-sass-authoring), I
hinted at a growing concern with CSS performance. Among those pioneering
ways to approach the issue of maintainable-yet-efficient CSS is [Nicole
Sullivan](http://twitter.com/stubbornella), who is perhaps best known for her
open source [Object-Oriented CSS](http://oocss.org) project.

## Object-Oriented? CSS?

I have to admit that when I first checked out out OOCSS, I guffawed.
While it's true that, at first blush, CSS does not have many of the
traditional features of a genuine OO programming language, Nichole has
been exploring ways in which CSS' inheritance/cascade can be analogous
to OO concepts.

It's taken me quite some time to come around. There are things in the
OOCSS code base that seem to fly in the face of commonly-accepted CSS
best practices. It is precisely this kind of resistance in the community
that must have prompted Nichole to deliver her latest talk this year at
Webstock, entitled [Our Best Practices Are Killing
Us](http://www.webstock.org.nz/talks/speakers/nicole-sullivan/css-tools-massive-websites)
([slides](http://www.slideshare.net/stubbornella/our-best-practices-are-killing-us)).

## Semantics Pitfall

For the past several years, I have been painstakingly crafting HTML and
CSS under the banner of "best practices." Currently, one such practice
has been to aggressively scope CSS by content type.

![Pitfall Harry](/images/2011-07-17-oocss-for-great-justice/pitfall.jpeg)

Martin Sutherland [describes this
approach](http://sunpig.com/martin/archives/2008/10/07/maintainable-css-modular-to-the-max.html)
better and more succinctly than I could, but just recently [recanted in
favor of
OOCSS](http://www.sunpig.com/martin/archives/2011/06/25/oocss-and-html-semantics.html).
Martin had watched Nichole's talk, and it appears to have had the same
effect on him as it had on me. He writes:

{% blockquote Martin Sutherland http://www.sunpig.com/martin/archives/2011/06/25/oocss-and-html-semantics.html %}
One of the hardest hurdles to leap in coming to like OOCSS was the
somewhat heretical notion of adding "non-semantic" container elements
and apparently "presentational" classnames to my HTML.
{% endblockquote %}

Martin [goes on to
propose](http://www.sunpig.com/martin/archives/2011/06/25/oocss-and-html-semantics.html)
(and I think he's right on target here) that there are multiple layers
of semantics that belong in HTML:

1. Structural semantics (core HTML elements)
2. Ontological semantics (domain-specific meaning beyond core HTML)
3. Visual semantics (representing visual intent)

Lest we forget, one of the core purposes of CSS **is** to communicate
the [visual
semantics](http://www.stubbornella.org/content/2010/06/12/visual-semantics-in-html-and-css/)
of an HTML document. Many developers, myself included, have written many
projects' worth of CSS that attempts to use only core HTML (structural)
and domain-specific (ontological) semantics, while sidestepping the role
that visual semantics ought to play, out of fear that we'd be
introducing the ultimate evil into our source code, *presentational
classnames*.

It is becoming more clear to me that we should not avoid the question of
visual semantics in our CSS. The result of our having done so is visual
style that is tightly bound to the content.

Now, hearken back to the primitive days of yore when it was commonplace
(and accepted) to see `<font>`, `<u>`, and `<center>` tags in HTML
source.  It's another example of presentation coupled with content,
only in a more obviously wrong form.

**Is the intertwining of visual style and content not one of the
problems, nay, *the* problem, that CSS was intended to address in the
first place?**

It seems to me that many of us are doing it wrong when we refuse to
implement some level of visual semantics in our CSS and HTML.  Because
of the belief that we're following "best practices," we are robbing
ourselves of opportunities to abstract out common styles into reusable
patterns, and our projects suffer for it.

## For Great Justice

I've watched as some developers have begun to [wrestle with the concepts
of
OOCSS](http://lazukars.com/post/7300553347/brain-vs-object-oriented-css)
vs. what we have come to know as the industry standard practice of
writing CSS in recent years.

Watch Nichole's OOCSS talks, and allow them ample time to marinate.
Check out the [source](http://github.com/stubbornella/oocss) of the
[OOCSS project](http://oocss.org). I'm still in process myself, as I
continue to look for ways to strike the delicate balance between
*appropriate* visual semantics in CSS and blatantly presentational
naming.

![ZeroWing](/images/2011-07-17-oocss-for-great-justice/ZeroWing.png)

Not everything in OOCSS is a great idea to implement verbatim.  Remember
that it is proof of concept and a collection of patterns, not a CSS
boilerplate. I am still mildly horrified every time I see how `<b>` tags
are used for rounded corners in the [module
example](http://oocss.org/module.html), but I remind myself that the
concepts are what make up OOCSS, not the specific implementation of the
examples.

Finally, I have linked to Martin's [article on
OOCSS](http://www.stubbornella.org/content/2010/06/12/visual-semantics-in-html-and-css/)
in several places throughout this post. It really is required reading if
you author CSS as part of your day job.
