---
title: "VCS: and how I use git remotes"
type: docs
categories: devx, VCS
draft: true
---

Much (most?) software development uses a service/platform for collaboration sometimes referred to as a [software forge](https://en.wikipedia.org/wiki/Forge_(software), and we can see this especially in the open source world. Some are themselves open source software, some are completely proprietary, and some are somewhere in between.

At its very simplest a software forge is a hosting service for version control[^vcsgit] repositories, but typically includes functionality for some or all of the following:

* Code Review
* Role-Based Access Control
* Pipeline/Workflow tooling (for CI/CD)
* Bug/issue tracking
* Project management
* Artifact hosting

So which software do I use? Which service? And how to get the most win?

* Proprietary/Commercial: [GitHub][1]? [BitBucket][5]? [GitLab][2]? Something else?
* Free/FOSS: [Forgejo][3]? [SourceHut][4]? None[^none]?

I have used all of the Proprietary/Commercial options above extensively in my professional work and github/gitlab/forgejo/other for personal purposes.

One of the major problems (specifically in software development with respect to forges, but also more generally) that I have seen is that while almost every modern SWE uses version control for application development and often for infrastructure development, it is also _very_ common that SWE project repos themselves are not managed with the same diligent "Everything as Code" approach, which seems foolish to me:

- the tooling is available and generally fairly mature
- it's not very different from other IaC efforts, not much to learn
- the consistency that it provides is especially valuable when you have many repositories/projects

TODO: manyrepos

## How do I organize _my_ Projects?

* I run a private forgejo instance which is the primary forge for everything that I write for myself, for everything where I get to choose, _unilaterally_, whatever suits me. Such as this website.
* The "cleaned up for publication" repository is usually at github.com, at least partly because their GitHub Actions and GitHub Pages functionality is useful to me. But GitHub Pages is only available if the repo is public.

### Git Remote

The remote is usually named for the forge, but the special terms `origin` and `upstream` are reserved:

* I use `origin` for the remote that I use as the default remote for that project
* I use `upstream` for projects that I did not originate - it points to wherever I cloned the repository from in the first place.

```gitconfig

```

[1]: https://github.com
[2]: https://gitlab.com
[3]: https://forgejo.org
[4]: https://sourcehut.org
[5]: https://bitbucket.org/
[^none]: [Here](https://www.chiark.greenend.org.uk/~sgtatham/quasiblog/git-no-forge/) is an argument from Simon Tatham (of PuTTY fame) for "no forge, thank you!". See also the [HN discussion](https://news.ycombinator.com/item?id=43272275)
[^vcsgit]: I have, other the years, used most of the major FOSS VCS software (rcs,cvs,subversion,svk,bzr,hg) but now it's just all git, all the time.
