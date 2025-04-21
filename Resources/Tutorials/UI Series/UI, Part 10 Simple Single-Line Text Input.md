---
tags:
  - ui
  - blog
created: 21.04.2025
up: "[[UI Series Table of Content]]"
prev: "[[UI, Part 9 Keyboard and Gamepad Navigation]]"
---
[https://www.rfleury.com/p/ui-bonus-1-simple-single-line-text?utm_source=substack&utm_campaign=post_embed&utm_medium=web](https://www.rfleury.com/p/ui-bonus-1-simple-single-line-text?utm_source=substack&utm_campaign=post_embed&utm_medium=web)

### Many custom UIs don't implement common keyboard shortcuts in their single-line text fields. It doesn't have to be this way - let's look at a way to simplify the problem.

It was a sunny afternoon.

There I was: optimistic, and ready to take on the software world. I was encouraged by the vast number of from-scratch projects implemented without bloated, proprietary frameworks, by developers who weren’t afraid to understand the physical machine that they were working on.

I downloaded one of these projects to give it a whirl. I was playing around with the custom user interface and using it to build some data. Feeling happy about it, I went to save my data to disk. The user interface prompted me for a file path. I typed a filename—“`ryans ptojecr`”—“Oops! Made a mistake. I meant to type _project_. Let me hit _Ctrl + Backspace_ and try that again.”

Immediately after this keypress, on the screen it showed me “`ryans ptojec`”. The user interface programmer has not implemented _Ctrl + Backspace_.

I became grumpy. That is the moment that I knew I had to write this post.

## A Well-Earned Reputation

Custom, from-scratch user interfaces have earned the reputation for lacking a number of standardized features. This reputation is well-earned. I won’t defend _all feature standardizations_, but some of them exist for a reason. A great example is the set of _standard keyboard shortcuts_ within a text field, which a substantial percentage of current computer users are familiar with. They rely on these shortcuts for quickly communicating their intent through a user interface.

Arguments I’ve heard for lacking support for such shortcuts often include a perception of the problem being very complex, and that it’d require more code than practically required.

I know from experience that this is not the case. I won’t claim that I’ve implemented _literally all_ shortcuts—there are a lot of them—but I’ve implemented a fairly large subset of them, and the problem can be simplified such that it requires very little code.

Let’s take a look at how.

## Getting Our Bearings

First, I’d like to list all of the behaviors that I’ll cover in this post. It won’t be all of them, but it’ll be most of the common ones, and the implementation I provide in this post will be easily extendable to support more.

Let’s first define some terms, so that each behavior can be well-defined:

- _Point_ — Some position within the text—exists between characters.
    
- _Cursor_ — A point within the text that changes directly with user input, to position new insertions, deletions, or selections.
    
- _Selection_ — A _contiguous range of points_ within the text that the user actively changes in order to delete, copy, or replace chunks of text at once.
    
- _Word_ — Application-specific; some chunk of text that is naturally separated from other words. In a program in which you modify natural language text, this may be alphanumeric chunks of text separated by whitespace. Code editors tend to have slightly different rules—for example, they will consider whitespace, but also characters like `_`, `/`, or other symbols to also break words apart.
    

With that out of the way, here are all of the behaviors I’ll cover:

- _User Presses Right_ → If there is no selection active, the cursor moves one character to the right. If there _is_ a selection active, the cursor is moved to the _maximum side_ of the selection range.
    
- _User Presses Left →_ If there is no selection active, the cursor moves one character to the left. If there _is_ a selection active, the cursor is moved to the _minimum side_ of the selection range.
    
- _User Presses Shift + Right_ → The current selection—including if it’s empty—is kept, and the selection endpoint defined by the cursor position is moved to the right.
    
- _User Presses Shift + Left_ → The current selection—including if it’s empty—is kept, and the selection endpoint defined by the cursor position is moved to the left.
    
- _User Presses Ctrl + Right_ → The current selection is removed. The cursor moves to the next _word beginning_, where _word_ can be defined by the application. There is not a fully standard behavior here—code editors, for example, will consider words to begin and end at a different place than your web browser address bar does.
    
- _User Presses Ctrl + Left_ → The current selection is removed. The cursor moves to the _previous beginning_ of a _word_.
    
- _User Presses Ctrl + Shift + Right_ → The current selection—including if it’s empty—is kept, and the selection endpoint defined by the cursor position is moved to the next _word beginning._
    
- _User Presses Ctrl + Shift + Left_ → The current selection—including if it’s empty—is kept, and the selection endpoint defined by the cursor position is moved to the previous _word beginning_.
    
- _User Presses Backspace_ → If there is no selection, the character to the left of the cursor is deleted. The cursor moves back one character. If there _is_ a selection, the entire selection is deleted, and the cursor moves to the beginning of the previous selection range, and the selection is removed.
    
- _User Presses Delete_ → If there is no selection, the character to the right of the cursor is deleted. The cursor stays in the same position. If there _is_ a selection, the entire selection is deleted, and the cursor moves to the beginning of the previous selection range, and the selection is removed.
    
- _User Presses Ctrl + Backspace_ → If there is no selection, the range from the cursor’s position to the closest _word beginning_ to the _left_ is deleted. The cursor is moved to the minimum position of the deleted range.
    
- _User Presses Ctrl + Delete_ → If there is no selection, the range from the cursor’s position to the closest _word beginning_ to the _right_ is deleted. The cursor is moved to the minimum position of the deleted range (in this case, it does not move).
    
- _User Presses Home_ → The cursor moves to the beginning of the line. The selection is removed.
    
- _User Presses End_ → The cursor moves to the end of the line. The selection is removed.
    
- _User Presses Shift + Home_ → The cursor moves to the beginning of the line. A selection is formed from the cursor’s previous position to the beginning of the line.
    
- _User Presses Shift + End_ → The cursor moves to the end of the line. A selection is formed from the cursor’s previous position to the end of the line.
    
- _User Presses Ctrl + C_ → The selection is copied to the clipboard. The current selection remains.
    
- _User Presses Ctrl + X_ → The selection is copied to the clipboard. It is then deleted, with the cursor then moving to the minimum position of the selection.
    
- _User Presses Ctrl + V_ → If there is a selection, the selection is deleted, and the cursor moves first to the minimum position of the selection. Whatever textual content in the clipboard is inserted at the position of the cursor.
    
- _User Presses Textual Character_ → If there is a selection, the selection is deleted, and the cursor moves first to the minimum position of the selection. A new character is inserted at the position of the cursor. The cursor moves to the position after the newly-inserted character.
    

That’s a lot! It may seem daunting to support all of these behaviors. But, we can easily support the entire list by finding simple building blocks that are _compositional_—meaning, they can be combined in many ways—that allow us to _express_ any item in the list.

Let’s break the problem down.

_(The remainder of this post is for paid subscribers only)_