---
title: "Tips: Vim"
date: 2025-09-06
modified: 2026-03-21T18:51:28
categories: productivity
tags: vim, editor, tips
type: docs
draft: false
sidebar:
  open: true
---

I "grew up" as user of unixlike systems while working at cmu.edu and so I mostly used XEmacs because that was the editor of choice for the more experienced folk I was learning/copying from[^zephyr]. Although back then there were plenty of machines that only provided vi/vim, so I learned the basics. I had a period of a couple of years or more where I mostly used Emacs for prose/code and mostly used vi(m) for quick edits to configuration files, etc.

At some point, I decided that I would be well served to learn how to work reasonably well with "both"[^vimacs] the editors so I ditched Emacs "for a while" and started to learn more than the basics for vim. And it turned out, I never went back - not because vim was better, nor even that it was better for me, but because I didn't want to bother with another migration/learning curve.

### Vim script function for auto-updating modification times in docs

When I'm writing docs, I'm likely using Markdown or AsciiDoc(tor), and I usually want a metadatum to show when it was last modified, so I do not have to rely on filesystem mtime[^stat].


This Vim script function allows vim to auto-update the datetimestamp:
* For Markdown (with or without Hugo-style frontmatter) using `^modified:...`
* For AsciiDoc, there's an extra preceding colon, so using `^:modified:...`

```vimscript
fun DocsLastMod()
  " this conditional looks over the whole file or the first 10 lines whichever is less. This is enough because the
  " the 'modified' datetime should in the first few lines of the frontmatter header
  if line("$") > 10
      let endline = 10
  else
      let endline = line("$")
  endif
  let searchpattern = '^modified:.*$'
  " this conditional helps check whether this is Markdown for Hugo with frontmatter
  " because we want to write a different datetime for Hugo/non-Hugo
  if match(getline(1), '---') == 0
    let replacepattern = 'modified: ' . strftime("%Y-%m-%dT%H:%M:%S")"
  else
  " Markdown without Hugo frontmatter
    let replacepattern = 'modified: ' . strftime("%Y-%m-%d %H:%M")"
  endif
  if &filetype ==? 'asciidoctor'
    let searchpattern = '^:modified:.*$'
    let replacepattern = ':modified: ' . strftime("%Y-%m-%d %H:%M")"
  endif
  " this conditional ensures that the searchpattern has been found in the first few lines
  if match(getline(1,endline), searchpattern) >= 0
    execute "1," . endline . "s/" . searchpattern ."/" . replacepattern . "/"
  endif
endfun

" Markdown
autocmd BufRead,BufNewFile *.mkd,*.md setfiletype markdown
autocmd BufWritePre,FileWritePre *.md   ks|call DocsLastMod()|'s

" AsciiDoc
autocmd BufRead,BufNewFile *.adoc setfiletype asciidoctor
autocmd BufWritePre,FileWritePre *.adoc   ks|call DocsLastMod()|'s
```

There is some obvious room for improvement, especially DRYing it up, but this works and is clear to me - which is good because I don't often write vimscript.

## See Also

* My `.vimrc` in the dotfiles repository.

[^stat]: On (somewhat) modern unixlike systems, stat(1) will show details about a file. The exact set of data available will depend on the filesystem, but modification time(mtime) and "birth time" (aka "creation time") are available on at least: ext4, HFS+, NTFS, XFS, btrfs, zfs. And that covers everything I've dealt with in recent memory.

[^zephyr]: I _think_ because XEmacs had (better?) support for Zephyr, the CMU/MIT/etc. text-based messaging system.

[^vimacs]: I never really understood the partisan squabbles - they're both perfectly cromulent tools. And back when I was first getting into tech, they were both miles better than any of the other commonly available alternatives.
