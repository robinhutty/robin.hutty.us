---
title: Clean up git branches by date
date: 2025-05-21
type: blog
---

One of those tiny conveniences that I've meant to discover long ago, but finally got to.

My work (and my fun), both code and prose, is almost entirely in the git VCS and most of my Work is in git repositories that are shared with others.

Mostly, I tend to use topic branches and push them up to a central repository (gitlab/github/forgejo) and largely when these branches merge to the main branch, they are deleted from the central repository.

But locally? Yeah, I'm not so tidy. So once in a while, I get around to cleaning up some of the stale branches. So can I come up with a good proxy for ordering by 'staleness'? How about 'reverse sort by last commit date'? Sounds good.

```shell
$ git branch --sort=-committerdate --format='%(committerdate:short) %(refname:short)'
2025-05-21 robin/experiments
2025-05-21 alice/update-dependencies
2025-05-20 bob/intro-docs
...
2020-03-01 robin/first-attempt
```

This allows for nice one-liners that further filter this list and can then be passed to `git branch -D` for [mass] branch deletion.
