---
layout: post
title: "Developers Are Artists"
date: 2012-10-07 11:53
comments: true
categories: [code]
---

Software development is, in part, an artistic practice. As a developer, your
brush is a keyboard; your canvas, a text editor. You derive satisfaction from
crafting solutions to real problems, and it is your creativity that brings the
solutions to life.

<!-- more -->

## Beautiful Code

Development is necessarily a balancing act of competing concerns:

* Performance
* Scalability
* Maintainability
* Usability
* Accessibility
* **Beauty**

Yes, we care very much about beauty in our code, but unlike most other
concerns, it is subjective by nature. Beauty is very much in the eye of the
beholder.

Teaching how to design beautiful code is as challenging as teaching "the
practice of software development." Each of us is left to ourselves to figure it
out, either by following the example of others, or stumbling upon it ourselves
in a blinding aura of caffeine-induced revelation. More often than not, at
least in my experience, it's the former.

### So&hellip; Artists?

As tempting as it is to draw upon the analogy of being "underappreciated in our
time," it is noteworthy that developers often experience a similar creative arc
as artists of other disciplines. A great deal of code makes its initial
appearance as a "s\*\*\*\*y first draft," to quote Anne LaMott. Before that, it
may even have begun its life as a simple whiteboard doodle. Eventually, we
revise and refactor that first idea as we gain a fuller understanding of the
problem, and the best code eventually emerges. More often than not, it also
happens to be more elegant code, the rough edges having been smoothed out, and
the redundancy reduced. If all goes well, the developer can put down her brush
at this point and take a step back to take in the full scope of her work.

{% codeblock lang:sass %}
$break-small: 320px;
$break-large: 1024px;

@mixin respond-to($media) {
  @if $media == handhelds {
    @media only screen and (max-width: $break-small) { @content; }
  }
  @else if $media == medium-screens {
    @media only screen and (min-width: $break-small + 1) and (max-width: $break-large - 1) { @content; }
  }
  @else if $media == wide-screens {
    @media only screen and (min-width: $break-large) { @content; }
  }
}

.profile-pic {
  float: left;
  width: 250px;
  @include respond-to(handhelds) { width: 100% ;}
  @include respond-to(medium-screens) { width: 125px; }
  @include respond-to(wide-screens) { float: none; }
}
{% endcodeblock %}

In the example above (from [The Sass Way](http://thesassway.com/intermediate/responsive-web-design-in-sass-using-media-queries-in-sass-32)),
the `respond-to` Sass mixin provides a more attractive way to specify @media
queries in CSS, abstracting away some of the ugliness for the author.
Obviously, abstraction isn't always the best course of action, but oftentimes
the biggest reason for doing it is to end up with a cleaner codebase.

## Beauty often takes care of itself.

We must remember that beauty in code is a means, not an end, to our
satisfaction in development work. Most of us provide our services for hire. In
practical terms, each of those other competing concerns has a real link to
business value, and will tend to trump "beauty" when the two are in conflict.
However, I find that if everything else properly accounted for, beauty will
often take care of itself in the process.

We've been talking about this for a while now.

* 2007 &ndash; O'Reilly publishes the book [Beautiful Code](http://shop.oreilly.com/product/9780596510046.do), a collection of essays by notable members of the profession on how they approach programming problems in ways they deem beautiful.
* 2008 &ndash; Jeff Atwood proclaims that [Code Isn't Beautiful](http://www.codinghorror.com/blog/2008/02/code-isnt-beautiful.html) in reaction to O'Reilly's book, finding beauty not in the code itself, but in the ideas and algorithms beneath the surface.
* 2009 &ndash; Chris Coyier describes [What Beautiful HTML Code Looks Like](http://css-tricks.com/what-beautiful-html-code-looks-like/), focusing on how beauty in HTML is derived from craftsmanship.
* 2010 &ndash; Martin van Emden argues [In Defense of Beautiful Code](http://vanemden.wordpress.com/2010/10/05/in-defense-of-beautiful-code-2/), demonstrating that there are ways to write the same code that are more beautiful than others.
* 2011 &ndash; Alberto Gutierrez discusses [The Obsession With Beautiful Code](http://www.makinggoodsoftware.com/2011/03/27/the-obsession-with-beautiful-code-the-refactor-syndrome/), underscoring the dangers of pursuing beauty for beauty's sake.

Gutierrez' post particularly resonates with me, despite its being the least
complementary to my point of view, because I have observed many of the
tendencies he lists in myself as well as in many of my peers. Developers need
to be aware that subjective coding style can be conflated with beauty on
occasion, and it often has little to do with code quality. However, I do take
issue with his claim that [all code is crap](http://www.makinggoodsoftware.com/2009/11/09/the-four-golden-rules-to-be-a-better-software-developer/).

While it is true that much of what we work on is, at best, *ephemeral* even if
it does end up seeing the light of day, what we gain in experience will stay
with us for future opportunities to create beautiful code.
