---
title: "Bootstrapping Infrastructure: VCS for TF State"
date: 2025-09-07
modified: 2025-09-08T07:46:18
categories: infrastructure
tags: IaC
type: docs
draft: false
---

I use OpenTofu/Terraform(TF)[^tf] to provision/configure my infrastructure, etc. Mostly because an "Infrastructure As Code"[^CCA] approach is a _sine qua non_ for me[^IaC-benefits].

* If I use an "Infrastructure As Code" approach for all my projects in order to have programmatic control over all the things, that should include my VCS repositories, right?
* TF[^other-IaC] needs to store a State file[^state]. The State file contains information about the current state of the Resources that TF wants to manage in order to determine what needs to be changed.
* In the default/trivial/minimal case, TF will use a local file to store State which works but has limitations; what happens when I want to work with this State from a different machine? Or if there is a team? Should I sneakernet it? No, of course not. And so all these technologies include functionality that supports Remote State, and most of them support different kinds of backends for storing these States.
* But these backends _are_ infrastructure. So I should manage them with TF. And now I have a bootstrapping problem.

But git is a distributed VCS, so how about I use git as my backend for TF State? That way I can IaC my IaC - see [`terraform-backend-git`][3].

* create a local git repo, `tf-states`, with the directory structure I want to use to store the TF State for my GitHub repositories
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

* migrate your state into the newly configured repository

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

[1]: https://opentofu.org/docs/language/state/purpose
[2]: https://developer.hashicorp.com/terraform/language/state
[3]: https://github.com/plumber-cd/terraform-backend-git
