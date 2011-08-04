---
title: Responsible Sass Authoring
date: 2011-07-09 13:04
layout: post
categories: [css, sass]
---

In capable hands, [Sass](//sass-lang.org) can do amazing things for your
CSS. With Sass, you can use functions and variables that later get
compiled into valid CSS. This can greatly reduce code repetition and
the potential for mistakes.

For example, not that you would, but you could write some Sass like this:

{% include_code 2011-07-09-responsible-sass-authoring/example-1.sass %}

The resulting CSS might look something like this, depending upon how you
set your Sass output style preference:

{% include_code 2011-07-09-responsible-sass-authoring/example-1.css %}

Pretty cool, right? You can see that the `@extend` keyword can be a very
powerful tool, bringing a quasi-object-oriented paradigm to our CSS. The
same could be true for the nested selectors; you just have to refer to
`.article` once, and let the indentation take care of the scope for
you.

## CSS Bloat

{% pullquote %}
Over a year of using Sass every day at my day job has
taught me this: {" Just because Sass allows you to do something, doesn&rsquo;t mean it should be&nbsp;done. "}

Sass clearly offers many ways to make our lives easier as developers.
The benefits of using it are obvious and many. `@import` is awesome; it
lets us concatenate many development Sass files into one production CSS
file if we wish. Variables and mixins can be amazingly handy.

Selector nesting, `@extend`, and parent-selector (&amp;), while
incredibly useful when care is taken, can also be prone to generate
sub-optimal CSS when used carelessly, *especially* in cases where they
are used in combination. Long-term, CSS that is compiled from Sass that
abuses these features can quickly become extremely bloated and
challenging to maintain.  Let's take a look at some examples of some
really sad CSS. The examples I'm using are inspired by actual projects
I've worked on (with some key edits to protect the innocent).
{% endpullquote %}

*Note that I'm not blaming Sass itself for any of these problems. It's
quite simply that using some Sass features in a certain way can
inadvertantly amplify poor CSS design decisions.*

### Case 1

Sass selectors that are nested like this:

{% include_code 2011-07-09-responsible-sass-authoring/example-2.sass %}

will result in CSS descendant selectors that look something like this:

{% include_code 2011-07-09-responsible-sass-authoring/example-2.css %}

Yuck. I'm not going to go into why this is bad, but you don't have to
take my word for it; simply google "[CSS performance descendant selectors](//www.google.com/search?q=css+performance+descendant+selectors)"
and read up.

With Sass, this kind of CSS bloat is extremely easy to cause if you get
carried away with selector nesting. Observe the redundancy and the
verbosity of the generated CSS. If the developer had taken the time to
carefully construct his selectors, he could have avoided the need for
this many levels of descendants in his CSS.

To my great shame, that developer was yours truly.

### Case 2

This one combines the `@extend`, nesting, and parent-selector features
of Sass. `@extend` has been one of my favorite features of Sass ever
since it was added. It lets you inherit the properties of one selector
in another.  Here's how it can go awry, despite all the best intentions.
The Sass:

{% include_code 2011-07-09-responsible-sass-authoring/example-3.sass %}

In the Sass, let's assume I extended `.clearfix` in 100 separate
selectors at various places throughout my code. This is, unfortunately,
a very realistic use case for something as commonly used as a clearfix,
and as my project grows, it would eventually result in the following
generated CSS:

{% include_code 2011-07-09-responsible-sass-authoring/example-3.css %}

First, there is a nesting issue. Because I nested `&:after` inside an
empty parent selector, the generated CSS contains a huge list of
selectors with no properties, then a second huge list with the clearfix
styles. Plus, all these bogus selectors tend to clog up dev tools like
Firebug, as shown in this screenshot:

![@extend .clearfix Firebug hell](/images/2011-07-09-responsible-sass-authoring/extend-clearfix-firebug-hell.png)

Secondly, this could have all been avoided if I had been more pragmatic
and simply used `.clearfix` (or another class &ndash; I commonly use
`.group`) in the HTML to apply the CSS and make it semantically
meaningful at the same time. Using `@extend` is not the correct choice
for this particular pattern:

{% include_code 2011-07-09-responsible-sass-authoring/example-4.sass %}

{% include_code 2011-07-09-responsible-sass-authoring/example-4.html %}

## Sass =&gt; CSS

{% pullquote %}
I, for one, side with those who seek to balance CSS performance concerns
with maintainability concerns.
{" It&rsquo;s important to remember that no matter what you do in Sass, it will eventually end up as&nbsp;CSS. "}
Whether it does more harm than good will be up to you.

Ironically, though it might seem at first that we have veered too far
towards the maintainability side of the spectrum with our CSS, it turns
out that the status quo isn't terribly well suited for either
maintainability or performance.
{% endpullquote %}
