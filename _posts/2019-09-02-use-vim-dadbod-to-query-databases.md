---
title: Use vim-dadbod to query databases
layout: post
---

**TLDR:**
Query your databases from any vim buffer:

<img src="/assets/gifs/tldr-vim-dadbod.gif" class="img-fluid" alt="tldr animated gif with vim-dadbod">

<!-- more -->

# What is vim-dadbod

It is fairly simple [vim plugin](https://github.com/tpope/vim-dadbod) (from
the Tim Pope) to do database queries showing result in a separate buffer.

It has only one command `:DB` that does all the stuff needed.

**NOTE:** Of course to query specific databases you should have proper tools
installed.

<img src="/assets/gifs/vim-dadbod-usage.gif" class="img-fluid" alt="animated gif with vim-dadbod usage">


You can set up global `g:db` variable to omit database urls:

    let g:db = "postgresql://postgres:@localhost/postgres"

Or use buffer local `b:db` variable to do the same (but you can have per
buffer database setup):

    let b:db = "postgresql://postgres:@localhost/postgres"

With that set up you can just call `:DB` command:

<img src="/assets/gifs/vim-dadbod-usage-var.gif" class="img-fluid" alt="animated gif with vim-dadbod usage with variables">

It works! But setting `b:db` manually or use full `:DB url query` command is
not the most convenient thing to do...

So, let's fix it!

# Add some goodies... operator

I will use buffer local `b:db` variable to store our database url.

What I want to have is to run queries under cursor by pressing `<leader>db` + text object.

Let us see what is in the help: `:h map-operator`. There is an example how to
create operator, let us reuse and slightly change it:

```vim
"" operator mapping
func! DBExe(...)
	if !a:0
		let &operatorfunc = matchstr(expand('<sfile>'), '[^. ]*$')
		return 'g@'
	endif
	let sel_save = &selection
	let &selection = "inclusive"
	let reg_save = @@

	if a:1 == 'char'	" Invoked from Visual mode, use gv command.
		silent exe 'normal! gvy'
	elseif a:1 == 'line'
		silent exe "normal! '[V']y"
	else
		silent exe 'normal! `[v`]y'
	endif

	execute "DB " . @@

	let &selection = sel_save
	let @@ = reg_save
endfunc
```

The "meat" here is `execute "DB " . @@` line. It will execute `DB` command
with what was visually selected or used as text-object.

Let's add some plugs and mappings:

```vim
xnoremap <expr> <Plug>(DBExe)     DBExe()
nnoremap <expr> <Plug>(DBExe)     DBExe()
nnoremap <expr> <Plug>(DBExeLine) DBExe() . '_'

xmap <leader>db  <Plug>(DBExe)
nmap <leader>db  <Plug>(DBExe)
omap <leader>db  <Plug>(DBExe)
nmap <leader>dbb <Plug>(DBExeLine)

```

So `DBExe()` without parameters will set proper `operatorfunc` and return `g@`
command that should be run on a visual selection or text object.

Thus the third line, if you are curious, will set `operatorfunc`, return `g@`
and add a motion `_` to it. Which will effectively run `g@` on the current
line.


Example of using this operator:

<img src="/assets/gifs/vim-dadbod-usage-operator.gif" class="img-fluid" alt="animated gif with vim-dadbod usage with operator">


# Databases. Choose the one.

If you have more than 1 database to query, it would be quite cumbersome to
manually set `b:db` every time you want to query another database.

There is a new `:h popup-menu` in vim -- let us pick databases using it.

**NOTE:** It uses new popup menu stuff from vim (use recent vim built from source code
or for windows folks, there is a place with pre-built binaries), you can of
course use commands with autocompletion but I wanted to tinker with popups.

First of all, database info should be stored somewhere, let it be list of
dictionaries:

```vim
let g:dadbods = []
let db = #{
		\name: "DEV Stage",
		\url: "postgresql://stage_user:dummypassword@test.example.com/stage"
		\}

call add(g:dadbods, db)

let db = #{
		\name: "DEV Main",
		\url: "postgresql://main_user:dummypassword@test.example.com/main"
		\}

call add(g:dadbods, db)

""" TEST

let db = #{
		\name: "Test Postgres DB",
		\url: "postgresql://postgres:@localhost/postgres"
		\}

call add(g:dadbods, db)

```

**NOTE:** It is not really safe to store your credentials in your dotfiles
this is just for simplicity. Personally I have it in a separate
`mydadbods.vim` which is not anywhere on the web.


Having that list of our databases we can come up with a command to select
current database (for an active buffer):

```vim
command! DBSelect :call popup_menu(map(copy(g:dadbods), {k,v -> v.name}), #{
			\callback: 'DBSelected'
			\})

func! DBSelected(id, result)
	if a:result != -1
		let b:db = g:dadbods[a:result-1].url
		echomsg 'DB ' . g:dadbods[a:result-1].name . ' is selected.'
	endif
endfunc
```

Example usage:


<img src="/assets/gifs/vim-dadbod-usage-menu.gif" class="img-fluid" alt="animated gif with vim-dadbod popup menu">

# Done!

Usually I use DBeaver (which is by the way awesome) to explore the database
and do heavy sql stuff but for quick and dirty queries I tend to use
`vim-dadbod` more and more.


Have fun!
