---
layout: post
title: Vim list-item toggling fun
---

I do almost all my doc stuff in vim + asciidoctor and it includes various list
todo/done checks in a status-like documents.

You know, lists like this:

    - [x] This is done
    - [ ] This is not
    - [x] Ping this dude to ping that dude to provide bla bla bla

<!-- more -->

That might look like this in HTML:

<img src="/assets/images/2018-12-06_12-41-20-html-rendered-list.png" class="img-fluid" alt="HTML rendered list">

or in PDF:

<img src="/assets/images/2018-12-06_12-42-09-pdf-rendered-list.png" class="img-fluid" alt="PDF rendered list">



While vim is not emacs in terms of "you can do whatever you want with it", you
can do a lot with vim and its vimscript:

<img src="/assets/gifs/vim-list-fun.gif" class="img-fluid" alt="animated gif with Vim toggling list items">

It is not yet a separate plugin and still sits in my personal dotvim repo:
[pack/vim-list-man](https://github.com/habamax/.vim/tree/9425d29ac458c3f4e8d4c4c5164dd7c757e8d856/pack/habamax/start/vim-list-man/plugin). 

What is missing?

- [x] List toggling (operator like and with an ex command)
- [ ] List motion (like paragraph but list)
- [ ] List item motion (like sentense but list item)
- [ ] List item nesting (increase/decrease)
- [ ] List item reordering (including all subitems)
- ...
