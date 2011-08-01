---
layout: post
title: 10 Essential Vim Tricks
date: 2011-07-30 00:37
comments: true
categories: [vim]
---

Since I switched to Vim as my primary editor back in March of this year,
I have discovered a wealth of useful tricks that help me get things
done. Some of these techniques are truly indispensible, and should find
a place in any Vim user's quiver.

I'm listing these tricks here, and it's just as much for my benefit as
yours; I anticipate coming back here often to drink my own Kool-Aid. I
should also note that at this time, I'm nowhere near expert-level in my
Vim abilities, so this post will likely be of most benefit to those Vim
users who, like me, are still getting their feet wet with the editor.

## 1. Select Last Insert

After entering text in insert mode and exiting to normal mode, type: `` `[v`] ``.

Vim will visually select the region of text you just entered, which you
can then *yank* and *put* to your heart's content.

## 2. Visual Block Mode

You might know about Visual mode (v), and even Visual Line mode (V), but
do you know about Visual Block mode? You can enter it with `^v` (that's
Ctrl-v), and select text on the screen in columns and rows, rather than
by lines.  This can allow you to make some really powerful edits.

You've probably seen this in other editors. Imagine I have the following
list in a text file:

    Item 1. Laundry.
    Item 2. Dishes.
    Item 3. Groceries.
    Item 4. Rock out.

If I wanted to remove the word "Item" from each line, leaving me with a
numerical list, I could use `^v` in combination with Vim movement
commands: `^v3jeld`. That edit would leave me with:

    1. Laundry.
    2. Dishes.
    3. Groceries.
    4. Rock out.

## 3. Quick Letter Swap

If I accidentally type "teh" instead of "the", the natural reaction
would be to go back, select those two letters, and retype them, but
there's an easy trick for fixing any letter swap. Move the cursor to the
first letter of the swapped pair (in this case, the "e") and type `xp`.

This *cuts* the "e" and *pastes* it after the "h". It is faster than
selecting the text and re-typing the characters in the correct sequence.

## 4. The Glorious `.`

This is probably my most-used command in Vim. The lowly `` . `` is a force
to be reckoned with.

It repeats the last edit you made. Think about that. In this article, I
could do something like this to change some instances of "Vim" to
"Awesome", using `` n `` to cycle through the search results until I find
the ones I'd like to change:

    /vim<Enter>
    ciwAwesome<Esc>
    n.nnn.

## 5. Lorem

## 6. Lorem

## 7. Lorem

## 8. Lorem

## 9. Lorem

## 10. Lorem
