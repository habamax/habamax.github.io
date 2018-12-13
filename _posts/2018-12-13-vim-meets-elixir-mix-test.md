---
layout: post
title: Vim meets Elixir mix test
---

Out of curiousity have started following José Valim, creator of Elixir
programming language, in a series of his 
[twitch casts](https://www.twitch.tv/collections/YDM6eKu6bhV1Nw) solving 
[Advent Of Code](https://adventofcode.com/2018).

It is good! The language, the Erlang platform, José -- such a nice composition.
<!-- more -->

So, he uses tests a lot. Like alot.

- [x] This is indeed a very good practice. I want that too.
- [ ] It is convenient in GVim for Windows.

Nope, it is inconvenient. Built-in terminal -- inconvenient (you can't `:term
mix test`). Separate `cmd` window -- kind of OK, but still inconvenient. Plugin
[vim-test](https://github.com/janko-m/vim-test) -- might be convenient, but it
didn't work for me unfortunately.

After 2 evenings I have created my own fairly simple [plugin](https://github.com/habamax/vim-elixir-mix-test):

<img src="/assets/gifs/vim-elixir-mix-test.gif" class="img-fluid" alt="animated gif with Vim running mix test">

**PS**

It is so much easier for me to quickly come up with a "good enough" solution for whatever in vim compared to emacs.
