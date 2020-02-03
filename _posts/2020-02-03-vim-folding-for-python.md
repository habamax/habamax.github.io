---
title: Vim folding for a python
layout: post
---

For python one can use built-in folding based on indent `set foldmethod=indent` which is kind of OK:

<img src="/assets/images/2020-02-03_15-11-27.png" class="img-fluid" alt="folding with builtin indent">

But I think it should look more like this:

<!-- more -->

<img src="/assets/images/2020-02-03_15-13-21.png" class="img-fluid" alt="folding with expr ">

To have this folding, create a file in your 
[~/.vim/after/ftplugin/python.vim](https://github.com/habamax/.vim/blob/aa36f36b498f1d26a10a571dda22477176659fce/after/ftplugin/python.vim)

```vim
func! FoldIndent() abort
    let indent = indent(v:lnum)/&sw
    let indent_next = indent(nextnonblank(v:lnum+1))/&sw
    if indent_next > indent && getline(v:lnum) !~ '^\s*$'
        return ">" . (indent+1)
    elseif indent != 0
        return indent
    else 
        return -1
    endif
endfunc
setlocal foldexpr=FoldIndent()
setlocal foldmethod=expr
```

And that is all.

This file would be loaded for all python buffers applying new folding.


PS:

<img src="/assets/gifs/vim-python-folding.gif" class="img-fluid" alt="animated gif python folding">

