---
title: Using topic branches in git to ease integration pain
type: docs
draft: false
categories: Tools, VCS
---

If you want to participate in most public/FOSS projects using git,
you'll want to get comfortable with using topic branches. This is a
separate branch for a particular topic, even if you're making trivial
changes such as spelling. There are various advantages such as:

-   a separate branch is easily updated so you don't get behind what's
    public or have to fight hard to stay up to date
-   being separated out makes it easier to modify/compress your commits
    to ease the review burden for (upstream) maintainers
-   separation allows you to have multiple topics under development
    without so much chance of confusion

In a typical scenario, I have forked a repository on github and cloned
my fork locally, added the upstream repo:

```shell
    git clone git@github.com:robinhutty/testproject.git
    git remote add upstream https://github.com/<upstream_org|upstream_user>/<repository>.git
    git fetch upstream
    git merge upstream/main
```

Checkout a new branch, edit away, commit and push the new branch to the
origin:

```shell
    git checkout -b spelling_fixes
    git commit -m'spelling fixes' -a
    git push origin spelling_fixes
```

Then you can either have github create a pull request or
`git format-patch main`.

If your branchwork takes a while and there are upstream commits, then
you might want to consider rebasing to keep it up to date. :

```shell
    git checkout main
    git pull
    git checkout spelling_fixes
    git rebase -i origin/main
```

`git rebase --interactive` allows you to clean up your own branch to
make it more presentable upstream by:

-   removing superfluous commits (delete a line)
-   amending commit messages (pick-&gt;edit)
-   inserting commits
-   squashing commits (pick-&gt;squash to squash it into the previous
    commit)
-   reordering commits (reorder the lines)

Rebasing will stop to allow you to edit certain messages as you've
requested; to continue:

```shell
    git rebase --continue
```
