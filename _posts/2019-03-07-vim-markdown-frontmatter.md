---
title: Vim and markdown frontmatter
layout: post
---

Built-in markdown in vim (from Tim Pope --- vim hero) has no support for
pandoc frontmatter:

- no syntax highlighting;
- folding thinks frontmatter's end marker is the section fold beginning.

Like the picture shows:
<!-- more -->
![As is built in markdown in Vim](/assets/images/2019-03-07_12-30-29.png)

So, let's fix it!

## Highlight fronmatter

Create a file `~/.vim/after/syntax/markdown.vim`:

```vim
unlet b:current_syntax
syntax include @Yaml syntax/yaml.vim
syntax region yamlFrontmatter start=/\%^---$/ end=/^---$/ keepend contains=@Yaml
```

- This file will be sourced *after* vim set syntax for a markdown file
- It has commands to include `yaml` syntax file for a frontmatter region
  defined as something between 3-dashed lines.

Now reopen your markdown file to see:

![highlight frontmatter markdown in Vim](/assets/images/2019-03-07_12-38-51.png)

## Fix folding

Create a file `~/.vim/after/ftplugin/markdown.vim`:

```vim
function! MarkdownFold()
  let line = getline(v:lnum)

  " Regular headers
  let depth = match(line, '\(^#\+\)\@<=\( .*$\)\@=')
  if depth > 0
    return ">" . depth
  endif

  " Setext style headings
  let prevline = getline(v:lnum - 1)
  let nextline = getline(v:lnum + 1)
  if (line =~ '^.\+$') && (nextline =~ '^=\+$') && (prevline =~ '^\s*$')
    return ">1"
  endif

  if (line =~ '^.\+$') && (nextline =~ '^-\+$') && (prevline =~ '^\s*$')
    return ">2"
  endif

  " frontmatter
  if (v:lnum == 1) && (line =~ '^----*$')
	  return ">1"
  endif

  return "="
endfunction
```

Built-in markdown filetype plugin uses `MarkdownFold()` function to define folds.

We just copy-pasted it here with some changes:

- do not define a fold for a setext heading if there is no empty line above it;
- define a fold for a frontmatter.

Reload your markdonw file and it should look like:

![Fold frontmatter markdown in Vim](/assets/images/2019-03-07_12-51-25.png)


But wait! It doesn't look this fancy! I see dashes in frontmatter fold instead
of the title...

To fix this you have to setup your foldtext, add to your `.vimrc`:

{% raw %}
```vim
" My fancy foldtext
set foldtext=MyFoldText()
fu! MyFoldText()
	let line = getline(v:foldstart)

	" markdown frontmatter -- just take the next line hoping it would be
	" title: Your title
	if line =~ '^----*$'
		let line = getline(v:foldstart+1)
	endif

	let indent = max([indent(v:foldstart)-v:foldlevel, 1])
	let lines = (v:foldend - v:foldstart + 1)
	let strip_line = substitute(line, '^//\|=\+\|["#]\|/\*\|\*/\|{{{\d\=\|title:\s*', '', 'g')
	let strip_line = substitute(strip_line, '^[[:space:]]*\|[[:space:]]*$', '', 'g')
	let text = strpart(strip_line, 0, winwidth(0) - v:foldlevel - indent - 6 - strlen(lines))
	if strlen(strip_line) > strlen(text)
		let text = text.'…'
	endif
	return repeat('▧', v:foldlevel) . repeat(' ', indent) . text .' ('. lines .')'
endfu

```
{% endraw %}

It uses some unicode charachters your font has no support for. Just change them
to whatever you prefer.
