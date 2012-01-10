---
layout: post
title: "KSS and Middleman"
date: 2012-01-10 15:11
comments: true
categories: [css,ruby,sass]
---

I love the idea of building an "interactive style guide" for a website
design.  I really do. However, working in Agencyland, it can be
extremely difficult to budget enough time for this kind of tool when
seemingly higher-priority tasks pile up. Right or wrong, the utopian
vision of a living style guide often becomes a foregone luxury in the
throes of looming deadlines.

*Assumes the reader understands the basics of Ruby and CSS.*

<!-- more -->

## Middleman

For an upcoming project at [work](http://empathylab.com), I'll be
leading the effort to author static HTML templates for a different group
of software integrators to wire up to a complex back-end architecture
(rhymes with Shmeb-shmere).

We are also going to be building a whole front-end stack with CSS and
JavaScript to boot, so it makes sense to use a solid static framework to
take the pain away from developing a massive amount of templates without
any server-side help. The bottom line is that we need to be able to
export flat HTML files that work the same as the development templates.

Enter [Middleman](http://middlemanapp.com), a loosely-coupled, yet
carefully curated collection of Ruby libraries that all contribute to a
framework that is much greater than the sum of its parts. I won't go
into all of its features here&thinsp;&mdash;&thinsp;you can visit the
Middleman [site](http://middlemanapp.com) for everything you need to
know.

For our purposes, Middleman looks to be a robust platform on which to
build our static site.

## KSS

I am a closet fan of documentation specs like RubyDoc and JSDoc, but
they often introduce a lot of heavy baggage for developers.
[TomDoc](http://tomdoc.org) came on the scene not too long ago as a
really simple doc spec that Githubbers use in their in-house Ruby code,
and I was ecstatic when another Github guy, [Kyle Neath](http://warpspire.com),
released his own [KSS](http://github.com/kneath/kss) project in late
2011. I think I've subconsciously been yearning for this kind of CSS
documentation support for some time.

KSS is a documentation spec for CSS (one of the first of its kind that
I've heard of) that can be parsed by Ruby. This helps us do things
like&hellip; generating an interactive style guide. It's certainly not a
silver bullet, but KSS will lower some of the administrative barriers to
putting together helpful CSS documentation.

## KSS-ing Middleman

I thought it would be a good idea to try to port some of the
[KSS example app](https://github.com/kneath/kss/tree/master/example)
code over to my basic Middleman app. The example shows several
variations of form submit button styles, including states such as hover
and disabled, which makes for an excellent use case.

### The Styles

I'm using [Sass](http://sass-lang.com) and
[Compass](http://compass-style.org), with Middleman, so I did a quick
port of Kyle's example button CSS to Sass with some appropriate Compass
mixins. I found that KSS broke when used with the original terse Sass
format, but it did work fine with the newer SCSS format. I'm not
certain, but it's possible that this issue may be resolved in KSS in the
future.

Below is the result of porting the button CSS to `_button.scss`. The
comment block at the top is where the documentation magic happens. You
just describe what something is, specify a list of different states, and
then reference a numbered section of the style guide. That's it.

``` sass
    // Your standard form button.
    //
    // :hover    - Highlights when hovering.
    // :disabled - Dims the button when disabled.
    // .primary  - Indicates button is the primary action.
    // .smaller  - A smaller button
    //
    // Styleguide 5.1.1
    button {
      background-color: #f5f5f5;
      @include background-image(linear-gradient(#f5f5f5, #e5e5e5));
      border: 1px solid #ddd;
      border-bottom-color: #bbb;
      @include border-radius(3px);
      @include box-shadow(0 1px 4px rgba(0, 0, 0, 0.15));
      color: #666;
      cursor: pointer;
      font-family: "Helvetica Neue", Helvetica;
      font-size: 12px;
      font-weight: bold;
      line-height: normal;
      padding: 5px 15px;
      @include text-shadow(0 1px rgba(255, 255, 255, 0.9));

      &.primary, &.primary:hover {
        color: #fff;
        background-color: #8add6d;
        @include background-image(linear-gradient(#8add6d, #60b044));
        border-color: #74bb5a;
        border-bottom-color: #509338;
        @include box-shadow(0 1px 4px rgba(0, 0, 0, 0.2));
        @include text-shadow(0 -1px 0 rgba(0, 0, 0, 0.4));
      }
      &.smaller {
        font-size: 11px;
        padding: 4px 7px;
      }
      &:hover {
        color: #337797;
        background-color: #f0f7fa;
        @include background-image(linear-gradient(#f0f7fa, #d8eaf2));
        border-color: #cbe3ee;
        border-bottom-color: #97c7dd;
      }
      &:disabled {
        opacity: 0.5;
      }
    }
```

You actually *do* need to manage the numeric structure of the style guide
yourself. However, I appreciate that KSS makes you maintain control over
the meaningful aspects of organizing a style guide, while making it
possible to automate the tedious parts.

### The Config

For the style guide to look nice on the front end, there is also a small
amount of boilerplate CSS and JavaScript code to make some of the magic
happen, so I pulled those files in and called them from a separate
layout, `layouts/styleguide.erb`.

The KSS example app runs on Sinatra, and Middleman also basically runs
on Sinatra with some abstractions on top, so setting up the
configuration wasn't too tough. After adding `gem "kss", "~> 0.1.1"` to
the Gemfile and running `bundle install`, I added the following to
Middleman's `config.rb` file:

``` ruby
    require "kss"
    page "/styleguide/*", :layout => :styleguide do
      @styleguide = Kss::Parser.new('source/css')
    end

    helpers do
      # Generates a styleguide block.
      def styleguide_block(section, &block)
        @section = @styleguide.section(section)
        @example_html = kss_capture{ block.call }
        @_out_buf << partial('styleguide/block')
      end

      # Captures the result of a block within an erb template without spitting it
      # to the output buffer.
      def kss_capture(&block)
        out, @_out_buf = @_out_buf, ""
        yield
        @_out_buf
      ensure
        @_out_buf = out
      end
    end
```

For all style guide templates, the above configuration exposes a
variable containing a Ruby representation of all KSS-documented CSS in
the site (because KSS parses all documentation blocks in the CSS). A
couple of helpers are defined that the templates will have access to, in
order to handle the generated style guide block.

*Due to a bug in Middleman 3.0 beta, wildcard file paths did not set
local variables correctly. This bug should be resolved in 3.0 final.
Additionally, I suspect that the *`kss_capture`* helper is not exactly
optimal for use with Middleman, but I haven't taken the time to refactor
the example code beyond simply getting it working. It was originally
named *`capture`* in the example, but I renamed it to prevent conflicts
with the existing Middleman *`capture`* helper, which I would have
probably tried to use if it was available inside the config file.*

### The Templates

A partial template needs to be defined for rendering every style guide
section. The template will be used by the `styleguide_block` helper
defined in the config above: `styleguide/_block.erb`.

``` erb
    <div class="styleguide-example">

      <h3><%= @section.section %> <em><%= @section.filename %></em></h3>
      <div class="styleguide-description">
        <p><%= @section.description %></p>
        <% if @section.modifiers.any? %>
          <ul class="styleguide-modifier">
            <% @section.modifiers.each do |modifier| %>
              <li><strong><%= modifier.name %></strong> - <%= modifier.description %></li>
            <% end %>
          </ul>
        <% end %>
      </div>
      <div class="styleguide-element">
        <%= @example_html %>
      </div>
      <% @section.modifiers.each do |modifier| %>
        <div class="styleguide-element styleguide-modifier">
          <span class="styleguide-modifier-name"><%= modifier.name %></span>
          <%= @example_html.sub('$modifier_class', " #{modifier.class_name}") %>
        </div>
      <% end %>

    </div>
```

In `styleguide/buttons.html.erb`, I added the following template call:

``` erb
    <% styleguide_block '5.1.1' do %>
      <button class="$modifier_class">Default Button</button>
    <% end %>
```

Note that there is a single `button` element in the block. This is where
the magic happens, because it passes the `button` to `styleguide_block`,
which imports the `styleguide/block` partial for the style guide section.

The partial parses the KSS representation of the documented CSS for the
given section of the style guide (5.1.1). It prints out the section
number and SCSS filename, adds the documentation text to the page,
cycles through each CSS modifier of `button`, and generates a new
`<button>` tag for each modifier. The resulting section of the style
guide will look like this:

{% img /images/2012-01-10-kss-and-middleman/styleguide.png Style guide example %}

The original basic `button` element is listed first, followed by
accurate examples of all of its documented modifiers. The KSS JavaScript
file fakes the pseudo-selectors `:hover` and `:disabled`.

## Onward

For my project, at least, this is an encouraging proof-of-concept that I
plan to take forward. In the meantime, it looks like there is a generous
amount of potential in both Middleman and KSS, so be sure to check out
each of these projects.

If you have used either Middleman or KSS, what has your experience been
like thus far?
