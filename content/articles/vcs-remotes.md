---
title: "VCS: How I use multiple git remotes for writing and publishing"
date: 2025-11-25
modified: 2026-03-01T18:41:54
type: docs
categories: devx, VCS
tags: vcs, devx
draft: false
---

### Intention: Or how do I want my git remote usage to work?

1. A public repository for "published" (source) material. This also uses GitHub Actions/GitHub Pages to manage/serve a [public-facing website][3].
1. A private repository as a "central" repository for the sources for draft content, sources/content that I'm not yet ready to be exposed publicly. This also serves as an additional copy in case of disastrous loss.
1. I want to publish most of my personal work publicly, but only when I am ready for it to be seen - admittedly this is a "weak" approach to "developing in public"[^forever].

(The above refers specifically to the source - (markdown, etc.) content and CI/CD pipelines - for a single website; but I use this same pattern, more or less, for ~several~ many other repositories, most of which are more traditional code repositories.)

#### github.com[^obvs]

The "cleaned up for publication" repository is usually at github.com, at least partly because I'm happy to rely on their infrastructure/WAF/etc. while I'm getting going.

* GitHub Actions and GitHub Pages functionality is useful to me, but GitHub Pages is only available if the repo is public.
* GitHub does not support private branches in public repositories.

#### forgejo

I run a private [forgejo][2] instance which is the primary forge[^forge] for everything that I write for myself, for everything where I get to choose unilaterally, whatever suits me. Such as this website and my automation/IaC.

### Implementation

#### git remote

The [remote][1] is usually named for the instance/location of the forge, but I reserve some special keywords: `origin` and `upstream`:

* I use `origin` for the remote that I use as the default remote for that project
* I use `upstream` for projects that I did _not_ originate - it points to wherever I sourced/cloned the repository from in the first place.

But otherwise both the names of remotes and the names of my branches are not restricted by this process.

```shell
$ git clone git@forge.example.com:robinhutty/robin.hutty.us.git ~/rh/draft/robin.hutty.us
$ git remote add gh-draft git@github.com-me:robinhutty/robin.hutty.us-draft.git
$ git remote add gh-publish git@github.com-me:robinhutty/robin.hutty.us.git
```

This will:

* Clone an existing repository from _my_ forge into a local directory with a carefully conventional name (`~/<org_name_id>/draft/<repo_id>/`).
* Add a remote for the github-hosted private copy of the repository (for draft content).
* Add a remote for the github-hosted public copy of the repository (that uses GitHub Actions/Pages to publish a rendered version).

Which allows me to:

| Command                       | Result                                               |
|-------------------------------|------------------------------------------------------|
| `git push -u origin -a`       | Push _all_ branches to the remote that is _my_ forge |
| `git push -u gh-draft draft`  | Push the `draft` branch to the remote that is the private repository at github.com |
| `git push -u gh-draft`        | Push all branches to the remote that is the private repository at github.com |
| `git push -u gh-publish main` | Push the `main` branch to the remote that is the public repository at github.com (which will trigger a publication pipeline)|

Combined with a [fragment of SSH configuration](../vcs-identities/#sshconfig) and some [git configuration](../vcs-identities/), the overall result:

* is a writing/publishing process that supports multiple forges
* with (optionally) multiple identities and signed commits
* sophisticated draft/pre-publishing capabilities
* accommodates my CLI preferences, but supports others' ability to contribute via standard, more familiar VCS practices/techs (e.g. VSCode).
* All I have to do is setup each machine/repository correctly at initiation and respect the naming conventions that I have established. Importantly, I do not have to keep these details of this sophisticated setup/process in my mind while I am working on any particular project; I have shifted the cognitive burden of my functional requirements away from the daily workflow.
* Plus: it is not locked in to any particular walled garden and therefore lends itself to export/migration and self-hosting, increasing resilience and durability.

### TL;DR

#### Start a new repo?

1. [Provision the repository using TF](../IaC-bootstrap/):
    * add to the `.tfvars` at https://github.com/robinhutty/tf/blob/main/github/robinhutty.auto.tfvars
    * Apply the TF configuration:
        ```shell
        $ cd ~/<path-to-tf-repo>/github
        $ terraform-backend-git git \
            --repository git@github.com:robinhutty/tf-states.git \
            --ref main \
            --state rhu/tf/github/state.json \
            terraform -t $(command -v tofu) apply
        ```

1. Set up[^template] the local repo/checkouts

    ```shell
    $ mkdir ~/rh/draft/foo && cd ~/rh/draft/foo
    $ echo "" >> .gitconfig
    $ git init
    $ echo "# What is foo?" >> README.md
    $ git add README.md
    $ git commit -m'init' README.md
    $ git branch -M main
    $ git remote add draft git@github.com:robinhutty/foo-draft.git
    $ git remote add public git@github.com:robinhutty/foo.git
    $ git push -u draft --all
    $ git push -u public main
    ```

#### Add a new feature/article?

```shell
$ cd ~/<org_name_id>/draft/<repo_id>/
$ git checkout -b rh/feat1
$ # <write code/prose>
$ git commit -am'feat(feat1): Add feat1'
$ git push -u gh-draft
$ # <watch CI do its thing>. Including potentially push the commit to the non-draft remote!
```

TODO: See Also/hat-tip to https://flori.dev/reads/git-signing-and-multiple-identities/ and https://medium.com/@leroyleowdev/one-machine-many-identities-adding-effortlessly-switch-between-multiple-git-profiles-fd56a20bc181 and https://dev.to/victorbruce/managing-multiple-git-identities-a-seamless-workflow-for-personal-and-work-accounts-1kce

[1]: https://git-scm.com/book/en/v2/Git-Basics-Working-with-Remotes
[2]: https://forgejo.org
[3]: https://robin.hutty.us
[4]: https://github.com/robinhutty/templates

[^forever]: I love the idea of developing in public, fostering a culture of openness and community and so on. It's part of why I'm writing here at all. And I recognise and appreciate the power/benefits of the vulnerability, honesty, authenticity that it requires. But I also recognise the risks of "Anything published on the Internet lives forever (and can be used against you)". So yeah, but I get to choose :)
[^forge]: a [software forge](https://en.wikipedia.org/wiki/Forge_(software)), a collaboration service/platform for software development.
[^obvs]: The obvious choice, the dominant service - owned by BigTech, in this case MSFT.
[^ssh]: See my [SSH tips article](../tips-ssh/)
[^template]: Consider using (or creating if one does not already exist) a template for the repository based on its intended purpose: a TF module via boilerplate, an Ansible Role via Galaxy, individual document templates for prose in AsciiDoc/Markdown, Python/Go/etc. modules, etc. See [my template repository][4]
