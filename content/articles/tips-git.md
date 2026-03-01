---
title: "Tips: git tips and conveniences"
date: 2025-04-03
modified: 2026-02-20T12:13:30
type: docs
categories: VCS
tags: devx, tips, productivity, git
draft: false
---

Why am I bothering to write about Version Control Systems(VCS) when so much material has been written already by more dedicated, more knowledgeable, more capable writers than me? At least partly[^usual] because since there is so much writing about git (and the ecosystem of ancillary tools and workflows for how to use it/them well) that when I want to remind myself of some aspect or improve my usage, I start searching and get distracted by the firehose.

[^usual]: plus [my usual reasons](../../about/#about-this-sites-content).

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

### stash

> Use git stash when you want to record the current state of the working directory and the index, but want to go back to a clean working directory.

I will not try to improve the above sentence, from the [git docs](https://git-scm.com/docs/git-stash). I use `git stash` all the time - especially when I'm poking around reviewing someone else's PR/MR branch and it stimulates an idea for a change I want to make that should not be part of this/their changeset.

I particularly like it in the [form][6] "in order to keep my focus on the effort of this review, stash my changes, put them into a new branch which I can then work on later":

```shell
$ git stash
$ git stash branch new-branch-name
```

### Clean up

### Already-merged branches

Even if the filesystem usage of old, already-merged branches is not meaningful waste, let's get into the habit of cleaning them up occasionally because cognitive burden is The Enemy.

Here's the command that [Spencer Dixon][4][^dixon] ended up with:

```shell
$ git branch --merged origin/main | grep -vE "^\s*(\*|main|develop)" | xargs -n 1 git branch -d
```

I prefer that:

* it should be sorted by the date of the last commit
* so I can conveniently review and then delete deliberately

```shell
$ git branch --merged origin/main --sort=-committerdate --format='%(committerdate:short) %(refname:short)'| grep -vE "^[0-9]{4}-[0-9]{2}-[0-9]{2}\s*(\*|main|develop)"
```

This conveniently allows passing to various invocations of `head(1)` or `tail(1)` for additional filtering with criteria like "N most recent" or "N oldest".

[^dixon]: This was inspired by [@spencerldixon][4] and apparently the [CIA][5]!

#### Pending branches?

In the rush of development, sometimes I'm not allowed the luxury of only working on one changeset at a time - this show which local branches have not yet been merged to HEAD.

```shell
$ git branch --no-merged | grep -v '\\*\\|main\\|develop'
```

[1]: https://git-scm.com/book/en/v2/Customizing-Git-Git-Configuration
[2]: https://en.wikipedia.org/wiki/Alias_(command)
[3]: https://git-scm.com/book/en/v2/Git-Basics-Git-Aliases

[4]: https://spencer.wtf/2026/02/20/cleaning-up-merged-git-branches-a-one-liner-from-the-cias-leaked-dev-docs.html
[5]: https://wikileaks.org/ciav7p1/cms/page_1179773.html

[6]: https://git-scm.com/docs/git-stash#Documentation/git-stash.txt-branchbranchnamestash

## Configuration

There are so many knobs that enable [customization][1] of `git(1)`. Here are a few that I usually want (by `.gitconfig` section):

### core

* `editor` `vim --nofork`
* `excludesfile`

### user

* name
* email
* signingKey

### aliases

The shell (bash, zsh, powershell, etc.) provides [alias][2] functionality that replaces the typed text _before_ interpreting. `git(1)` also provides [alias][3] functionality.

Choosing which approach to use is mostly personal preference as far as I can tell, but I recommend choosing one and sticking with it everywhere. If you regularly work with git using several different shells, it's possibly worthwhile to keep your aliases for working with git confined to git because then you can use the same `.gitconfig`, regardless of the shell/environment.

Specify in the `[aliases]` section of your `.gitconfig`:

```gitconfig
[aliases]
# Reverse sort branches by last commit date, i.e. a proxy for "recent activity"
recent="branch --sort=-committerdate --format='%(committerdate:short) %(refname:short)'"
```

## Tools

* GitHub and GitLab both provide their own CLI tools for interacting with their services: [gh](https://github.com/cli/cli) and [glab](https://gitlab.com/gitlab-org/cli)
* [gcli](https://herrhotzenplotz.de/gcli/) is a CLI client to interact with multiple software forges such as forgejo, github, gitlab.
* [diffnav](https://github.com/dlvhdr/diffnav):  Git diff pager based on delta but with a file tree.
* [difi](https://github.com/oug-t/difi): another aid for reviewing.
* [ggc](https://github.com/bmf-san/ggc) provides a more interactive interface that `git(1)`.


## See Also

* https://www.free-tools.dev/blog/git
* Me on [multiple git identities](../vcs-identities/)
* Me on [sophisticated git remote usage](../vcs-remotes/) for private/public publishing

### worktree

TODO: look at https://blog.safia.rocks/2025/09/03/git-worktrees/ for combining worktree usage with --bare

### Branching & Merging workflows

* https://docs.github.com/en/get-started/using-github/github-flow
* https://trunkbaseddevelopment.com/
* https://nvie.com/posts/a-successful-git-branching-model/
