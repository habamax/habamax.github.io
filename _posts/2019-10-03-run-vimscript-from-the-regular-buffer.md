---
title: Run vimscript from the regular buffer
layout: post
---

There is a post on [Reddit](https://www.reddit.com/r/vim/comments/dcoms4/vote_should_there_be_a_normal_mode_inside_command/) stating:

> Since vim doesnt have a repl we cant use a buffer for editing and executing
> in real time

Well, I do it all the time:

<img src="/assets/gifs/vim-evalvim.gif" class="img-fluid" alt="animated gif with vimscript evaluation">

<!-- more -->

I use my own [vim plugin](https://github.com/habamax/vim-evalvim) to evaluate
vimscript from within any vim buffer.

It is fairly simple and just wraps around vim's built-in `:@<register>` that
executes `ex` commands holded in a `<register>`.

You can execute/evaluate current line, text object or visual selection.

Have fun!
