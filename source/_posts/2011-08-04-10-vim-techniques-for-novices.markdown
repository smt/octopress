---
layout: post
title: 10 Vim Techniques for Novices
date: 2011-08-04 08:37
comments: true
categories: [vim]
---

Since I switched to Vim as my primary editor back in March of this year,
I have discovered a wealth of useful tricks that help me get things
done. Most of these techniques are truly indispensible, and should find
a place in any Vim user's quiver.

I should also note that at this time, I'm nowhere near expert-level in
my Vim abilities, so this post is intended to share some commands,
patterns, and configurations with other Vim users who are still getting
their feet wet with the editor. Experienced Vimmers will already know
all this stuff, and more power to them. This is for the Vim n00bs out
there.

### 1. Open with Cursor at Last Edit Position

This one isn't so much a technique as it is a preference. If you make a
change to a file, and then re-open it later, the following snippet in
your .vimrc will position the cursor at the last spot you made an edit
prior to closing the buffer or Vim itself:

    autocmd BufReadPost *
    \ if line("'\"") > 0 && line("'\"") <= line("$") |
    \   exe "normal! g`\"" |
    \ endif

<!-- " -->

### 2. Visual Block Mode

You might know about Visual mode `` v ``, and even Visual Line mode `` V ``,
but do you know about Visual Block mode? You can enter it with `^v`
(that's `CTRL-v`), and then use Vim movement commands to select text
in a box of columns and rows, rather than by lines. This can allow you
to make some really powerful edits.

Other editors, such as TextMate, can perform a similar blockwise
selection using the Alt/Option key. Well, MacVim can do the same thing.
Hold down Alt or Option, and click/drag with the mouse. I want to
caution you against relying on this too much, though. Vim is designed to
reward you for keeping your fingers *on the keys* as much as possible.

### 3. Quick Letter Swap

If I accidentally type "teh" instead of "the", the natural reaction
would be to go back, select those two letters, and retype them, but
there's a simple way to swap any two adjacent letters:

Move the cursor to the first letter of the swapped pair (in this
example, the "e" of "teh") and type `xp`. This cuts the character the
cursor is on, and immediately pastes it after the next.

### 4. Repeat Last Edit (the magic dot)

The glorious `` . `` is a very powerful command, and probably my
most-often used in this list. Typing `` . `` in normal mode repeats your
most recent edit. It's dead simple to use, and just plain awesome in so
many contexts.

### 5. Move and Edit by Text Object

At the root of Vim's idiosyncratic design is a variety of ways to easily
move around a text file and edit it effortlessly:

`` w `` and `` b `` move the cursor forward and backward by *word*.    
`` ( `` and `` ) `` move the cursor forward and backward by *sentence*.    
`` { `` and `` } `` move the cursor forward and backward by *paragraph*.

You can combine text objects with counts and actions to perform very
powerful, concise edits.

One of my favorite commands is `ciw`, and the pnemonic for it is Change
Inside Word. If I can get my cursor *anywhere* in a word, it doesn't
matter which character it's on, a quick `ciw` will cut the whole word
and change to insert mode so I can enter new text in its place. No
awkward text selection or repetitive deletion keying. Just `ciw` and
type the new word.

### 6. Delete, Yank, and Paste a Line

`` dd `` will "delete" a line, which is the same as *cut*.    
`` yy `` will "yank" a line, which is the same as *copy*.    
`` p `` will paste at the line *below* the cursor.    
`` P `` or `SHIFT-p` will paste at the line *above* the cursor.

Any of these commands can be preceded by a numeric count: `10p` will
paste 10 times.

### 7. Easy Repeated Characters

Say I am working in Markdown format -- which I often am; these blog posts
are is written that way -- and I want to use an underline-style H2:

    This is my heading
    ==================

I would prefer not to have to type who-knows-how-many equals signs in a
row. How annoying. Luckily, something like this is dead simple in Vim.

After typing your header, exit to normal mode and, with your cursor
still on the line, type `yyp` to duplicate the line. The text now looks
like this:

    This is my heading
    This is my heading

At this point, your cursor should on the first character of the second
line. Type `v$r=` to visually select the text to the end of the line,
and replace all the characters with equals signs. That's it: the full
command is `yypv$r=`.

If you ever just want 80 stars in a row, say, to section off a part of
source code, you could just type `80i*<Esc>` -- easy.

### 8. Quick Find

`` / `` will switch to a mode that allows you to enter a search string
for the current buffer. If you have the proper settings in your .vimrc,

Vim will even do an incremental search. When you have the search term
you want, hit `<Enter>` and cycle forward through the search results *in
the buffer* with `` n ``, and backwards with `` N ``.

`` ? `` does the same thing as `` / ``, only in the reverse direction.
You can also use `` * `` to quick-search the word your cursor is
currently on.

For some sane defaults, here are some of my .vimrc search settings:

    set hlsearch   " Highlight search matches
    set incsearch  " Highlight matches as you type
    set ignorecase " Case-insensitive searching
    set smartcase  " ...but case-sensitive if expression has caps
    set wrapscan   " Set the search scan to wrap around the file

    " Press space bar to turn off search highlighting
    " and clear any message displayed
    nnoremap <silent> <Space> :nohl<Bar>:echo<CR>

### 9. Search &amp; Replace in a Buffer

Few of us can really consider ourselves experts with regular
expressions, but if you have ever used them for any kind of text
processing task, you realize how incredibly powerful (and yes, sometimes
befuddling) they can be. Regexes are used in Vim's native search and
replace.

It is necessary to specify a range when doing a search and replace, and
generally, you will want the action to be global, so you would type
`:%s/pattern/replacement/g` and hit `<Enter>`. Done. The `` % ``
represents the entire buffer, while the `s///g` structure is the
standard search and replace command syntax.

To search/replace within a given selection, first select your desired
range in visual mode, then hit `` : ``. Vim will pre-fill your range as
`'<,'>`, which means the current selection. Use the same `s///g`
syntax as before to restrict the action to the selection you specified.

The trailing `` /g ``? That's an option that I usually like to pass, as it
tells Vim to replace all matches, not just the first one.

### 10. Project Search &amp; Replace

It's a common complaint: project search &amp; replace -- an operation
that spans one or more files -- is one of the few glaring weaknesses of
Vim. Until there is better support for this feature in Vim itself, it is
possible to pull off in an unorthodox way: don't use Vim! Try this on
for size (from the command line):

    $ sed -i 's/pattern/replacement/' <files>

From within Vim, while in `` : `` command mode, you can use the same
command, only beginning with a `` ! ``. This pipes the command directly
to a shell session and does the business. Note the relative consistency
of sed's search &amp; replace syntax with Vim's native one.

Passing commands from Vim to external programs is common, and often
encouraged. Select some text and run the `:sort` command on it sometime
-- it actually calls the external `sort` program, which passes the
sorted lines back to Vim.

## My dotvim

I wouldn't consider [my Vim configuration](http://github/smt/dotvim) to
be anything special, but it's mine, and I am always tweaking it (you Vim
people know what I'm talking about). If you are curious about what I
might have in there, you can feel free have a look at it -- it's on
GitHub, of course, and all the plugins are set up as Git submodules with
Pathogen.

## Dive into Vim!

The nuts and bolts of how to use Vim are, of course, outside the scope
of this article. [VimCasts](http://vimcasts.org), [PeepCode](http://peepcode.com/products/smash-into-vim-i),
and Vim's built-in tutorial are at your disposal. That being said, I'm
willing to attempt to answer most questions you might have about Vim,
either on [Twitter](http://twitter.com/tudorstudio) or via the comments.
