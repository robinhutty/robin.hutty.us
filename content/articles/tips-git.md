---
title: "VCS: git tips and conveniences"
date: 2025-04-03
modified: 2025-10-30T17:58:18
type: docs
categories: VCS
tags: devx, tips, productivity, git
draft: true
---

Why am I bothering to write about Version Control Systems(VCS) when so much material has been written already by more dedicated, more knowledgeable, more capable writers than me? At least partly because since there is so much writing about git (and the like) that when I want to remind myself of some aspect or improve my usage, I start searching and get distracted by the firehose.

## Usage

### worktree

TODO: look at https://blog.safia.rocks/2025/09/03/git-worktrees/ for combining worktree usage with --bare

### Filtering git log

Filter the change history shown by `git log ...`:
  * by branch with `git log <branch_name>`
  * by author(`--author=...`) or committer(`--committer=...`)
  * by date (`--since=2025-04-03` or `--before=2025-09-03`)
  * by path

### Cloning repositories incompletely

Do you really need the entire history of a large repo that's been around for ages? Probably not, eh?

* `git clone --single-branch ...`: Clone only the history leading to the tip of a single branch
* `git clone --depth <n> ...`: Create a shallow clone with a history truncated to the specified number of commits. Implies `--single-branch`
* `git clone --shallow-since="2025-01-01" ...`: Clone commits since a specific date.

See https://git-scm.com/docs/git-clone and https://git-scm.com/docs/partial-clone for more.


## Configuration

### aliases

```shell
# Reverse sort branches by last commit date, i.e. a proxy for "recent activity"
alias grecent="git branch --sort=-committerdate --format='%(committerdate:short) %(refname:short)'"


```

## tools

* [gcli](https://herrhotzenplotz.de/gcli/) is a CLI client to interact with multiple software forges such as forgejo, github, gitlab.

## See Also

* https://www.free-tools.dev/blog/git
