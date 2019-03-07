---
title: Paste clipboard images to Asciidoctor document from Vim 
layout: post
---

More improvements for [vim-asciidoctor](https://github.com/habamax/vim-asciidoctor) plugin. 

Now you can paste images to the document from clipboard:

<img src="/assets/gifs/asciidoctor-pasteimg.gif" class="img-fluid" alt="animated gif with Vim pasting clipboard images">

* No documentation yet. 
* ~~Doesn't work for neovim for now as I use vim's job stuff there.~~ Works for neovim too now.
* External dependency (via settings) for GraphicsMagic (windows), `pngpaste` (OSX), and I am not sure what Linux should use.
