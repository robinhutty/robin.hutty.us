---
title: "Bootstrapping Infrastructure: VCS for TF State"
date: 2025-09-07
modified: 2026-02-28T16:40:59
categories: infrastructure
tags: IaC,vcs,tf
type: docs
draft: false
---

## Introduction

I use OpenTofu/Terraform(TF)[^tf] to provision/configure my infrastructure, etc. - mostly because an "Infrastructure As Code"[^CCA] approach is a _sine qua non_ for me[^IaC-benefits].

* If I use an "Infrastructure As Code"/"Everything as Code" approach for all my projects in order to have programmatic control over all the things, that should include my VCS repositories, right?
* TF[^other-IaC] needs to store a State file[^state]. The State file contains information about the current state of the Resources that TF wants to manage in order to determine what needs to be changed.
* In the default/trivial/minimal case, TF will use a local file to store State which works but has limitations; what happens when I want to work with this State from a different machine? Or if there is a team? Should I sneakernet it? No, of course not. And so all these technologies include functionality that supports remote State, and most of them support different kinds of backends for storing these States.
* But these backends _are_ infrastructure. So I should manage them with my IaC approach. So now we see a bootstrapping problem.

But git is a distributed VCS, so how about I[^I] use git as my backend for TF State? That way I can IaC my IaC; this is my usage of [`terraform-backend-git`][3] from [Dee Kryvenko][4]. It is a [TF Backend provider][6] that allows TF to store its State files in a git repository. See the [the author's explanation][5] for some reasoning.

## What are the goals?

Starting with only an account at a VCS Forge (github/gitlab/forgejo/etc.) and a token to allow programmatic access, I want:

- A git repository that contains/can contain TF State files with a conventional directory structure that makes sense for my TF projects.
- A simple workflow/process so that my TF projects all use the same approach for finding/using their TF State.

## Implementation

Each TF project("configuration") has a [root module](https://opentofu.org/docs/language/modules/#the-root-module), with [persisted state][1].

TF projects have their persistent state stored in a directory structure in the `tf-states` [repository][7] that uses the following pattern:

```
tf-states
└── Project Group
    └── the Project's (primary) TF provider
```

For example:

```
tf-states
├── rh
│   └── github
│   └── forgejo
└── swip
    ├── aws
    ├── github
    └── gitlab
```

## Usage

Upon starting[^boilerplate] a new TF project, add a new HCL file, `tf-backend-git.hcl`[^tbghcl]:

```hcl
git.repository = "git@github.com:robinhutty/tf-states.git"
# git.repository = "https://github.com/robinhutty/tf-states"
git.ref = "main"
# The path to the `state` below should follow the directory structure above
git.state = "rh/github/state.json"
```

Then use `terraform-backend-git` as a wrapper for terragrunt[^tggen]/opentofu/terraform:

```console
$ terraform-backend-git git terraform -t $(command -v tofu) init

```

[^tbghcl]: See [HCL Mode](https://github.com/plumber-cd/terraform-backend-git?tab=readme-ov-file#hashicorp-configuration-language-hcl-mode) in the `terraform-backend-git` documentation.
[^tggen]: This pattern allows lends itself to supporting [TACOS][10] software such as terragrunt - which can generate the `tf-backend-git.hcl` file.

## How to bootstrap?

1. Create a local git repo, `<path>/rh/github`, that will contain the TF project that manages GitHub for the `rh` effort - at least a root module, but it can call child module(s) if they're available. Might as well use [boilerplate][8], eh?
    ```console
    # for a TF project named `foo`
    git init
    echo "# foo" > README.md
    git add README.md
    git commit -m "Init foo"
    git branch -M main
    git remote add origin git@github.com:robinhutty/foo.git
    git push -u origin main
    ```
2. Write TF code to provision/configure the set of GitHub repositories and other resources for the `rh` effort.
3. At a minimum, that set should contain a repository for the `tf-states`! But to make this non-trivial, I'll also add another repository for my (child) tf-modules.
4. Run a TF `init` - which will install GitHub provider, etc.
5. Run a TF `apply` - which will provision/configure the GitHub repositories
6. Add a `tf-backend-git.hcl` referring to the new `tf-states` repository and the path to where the State file within the `tf-states` repository should live.
7. Run a TF `init -migrate-state`.
8. Run a TF `plan` - which should show no changes.


* create a local git repo, `tf-states`, with the directory structure I want to use to store the TF State for my TF projects
* add a readme, commit
* use terraform-backend-git to provision (at least) the `tf-states` repository:

    ```shell
    $ terraform-backend-git git \
        --repository /path/to/tf-states \
        --ref main \
        --state rhu/tf/github/state.json \
        terraform -t $(command -v tofu) init
    $ <snip> plan
    $ <snip> apply
    ```

* add the new (remote/GitHub) repository as an origin and push to it
	```shell
	   git remote add origin git@github.com:robinhutty/tf-states.git
	   git push -u origin main
	```

* migrate the state into the newly configured repository

    ```shell
    $ terraform-backend-git git \
        --repository git@github.com:robinhutty/tf-states.git \
        --ref main \
        --state rhu/tf/github/state.json \
        terraform -t $(command -v tofu) init -migrate-state
    ```

[^tf]: I've been using Terraform continuously since the v0.3.0 release in 2014 which added Modules(!). Like many of us, I've been switching to OpenTofu. I've started trying to remember to use "TF" when I'm talking about something that is not specific to either tool.
[^state]: This is necessary to determine the delta between the (current) reality of your Resources and your desired reality for those Resources (remember, this is Declarative IaC, right? The TF code specifies what _you want_, not what _to do_). See the material about TF State from [OpenTofu][1] and [HashiCorp][2].
[^CCA]: First we talked about "Configuration Management"(CM). I used tools like CFEngine, bcfg2, Chef, Puppet and various less well-known techs (including some homegrown by various employers and/or by myself). Sometime (in the mid 2010s? At least a decade after I was working with CM), the term "Continuous Configuration Management" came along. I prefer "... as Code" because, to me, it emphasizes the intention to have programmatic control to beget reproducibility.
[^other-IaC]: Other IaC technologies mostly have something similar - see [Pulumi State](https://www.pulumi.com/docs/iac/concepts/state-and-backends/), [SaltStack State](https://www.pulumi.com/docs/iac/concepts/state-and-backends/)
[^IaC-benefits]: I have written countless words on "Why IaC?" - mostly in (internal) email and mostly aimed at explaining to management or newbie techies. But okay, I should (will?) publish something with the benefits clearly explained and advice for mitigating the challenges. The main thrust will be "Reproducibility" because, in my opinion, almost everything flows from there.
[^I]: Relevant Environment Variables: SSH_PRIVATE_KEY, GIT_USERNAME, GITHUB_TOKEN. Also: [about multiple [git] identities](../vcs-identities/)
[^boilerplate]: When starting a new TF project/module, I like to use [boilerplate][8] or [cookiecutter][9] to help with consistency. See [Tips: OpenTofu/Terraform](../tips-tf/) and my [templates repository](https://github.com/robinhutty/templates).

[1]: https://opentofu.org/docs/language/state/purpose
[2]: https://developer.hashicorp.com/terraform/language/state
[3]: https://github.com/plumber-cd/terraform-backend-git
[4]: https://github.com/plumber-cd
[5]: https://github.com/plumber-cd/terraform-backend-git?tab=readme-ov-file#why-storing-state-in-git
[6]: https://opentofu.org/docs/language/settings/backends/configuration
[7]: https://github.com/robinhutty/tf-states
[8]: https://github.com/gruntwork-io/boilerplate
[9]: https://cookiecutter.readthedocs.io/
[10]: https://opentofu.org/docs/intro/tacos
