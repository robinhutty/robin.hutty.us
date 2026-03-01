---
title: "Tips: OpenTofu/Terraform"
date: 2025-11-21
modified: 2026-02-28T16:36:40
categories: IaC
tags: hcl, tf, tips
type: docs
draft: true
---

## Version Manager

Like many other sections of the software engineering world, IaC folk want a version manager to simplify the complexity of dealing with different versions.

I like [tenv](https://github.com/tofuutils/tenv). But if [mise-en-place](https://mise.jdx.dev) is already part of the overall environment/workflow for a project, it also caters to [opentofu](https://mise-tools.jdx.dev/tools/opentofu) and [terraform](https://mise-tools.jdx.dev/tools/terraform). And offers additional [hints](https://mise.jdx.dev/mise-cookbook/terraform.html).

## TF State Backend

There are many different [Backends][1] for [TF State][2]; selecting a backend should depend on the details of the use case:

- whether there are multiple individuals or even teams that need to work with it
- which TF Automation and Collaboration Software([TACOS][3]), if any.
- ...

[1]: https://opentofu.org/docs/language/settings/backends/configuration
[2]: https://opentofu.org/docs/language/state
[3]: https://opentofu.org/docs/intro/tacos

In general I feel like most non-trivial use cases use a remote backend, probably either the [http](https://opentofu.org/docs/language/settings/backends/http) backend or the [s3](https://opentofu.org/docs/language/settings/backends/s3) backend[^s3].

For a detailed demonstration of how to use git as a backend for TF State, see my article about [Boostrapping Infrastructure](../IaC-bootstrap) where I use TF to provision a git repository and then store the [TF State in that git repository][4].

[4]: https://github.com/plumber-cd/terraform-backend-git
[^s3]: Stores the state as a given key in a given bucket on Amazon S3 or compatible service such as [garage](https://garagehq.deuxfleurs.fr).

## DevX for TF

### Module boilerplate

- I like [boilerplate](https://github.com/gruntwork-io/boilerplate) from [GruntWorks](https://www.gruntwork.io/blog/introducing-boilerplate)[^gw]. Boilerplate that I have used for individual modules can be found in my [templates repository](https://github.com/robinhutty/templates).
- [cookiecutter](https://cookiecutter.readthedocs.io/) is a great, but more generic templating system for generating this kind of material. See [TerraformInDepth](https://github.com/TerraformInDepth/terraform-module-cookiecutter) for an example.

[tg]: https://terragrunt.gruntwork.io
[tt]: https://terratest.gruntwork.io

[^gw]: More famous for [Terragrunt][tg] and [Terratest][tt].

### pre-commit and pre-commit-opentofu

* tflint
* https://github.com/tofuutils/pre-commit-opentofu

### TF techniques

### That TF-managed Resource that is your dependency, is it Really Ready?

Sometimes, tofu/terraform (and their Providers that enable them to provision Resources) need a little help with ensuring that a Resource's dependencies are Really Ready when the Provider claims they are.

The reason why this mattered in the real world example[^eg] that made me write this down was so that I could collapse three Terragrunt codebases to two. Which meant that we could:
* reduce the cognitive burden of dealing with this codebase and the systems it provisions/manages
* eliminate several kLOC of nearly duplicated code over dozens of files, which previously we had to carefully curate/test
* eliminate whole classes of CI/CD Pipeline Jobs
* massively reduce the iteration time for both engineering this codebase and the operations/change management

[^eg]: The details of how the benefit came to be so huge are way beyond the scope of this article, would require considerable explanation of both the codebase in question and the purpose of the systems/environments that it manages. Fortunately, none of that is necessary - I want this as a record of how to use `null_resource` and `lifecycle`.

#### Using a `local-exec` provisioner to wait

```hcl
resource "aws_eks_fargate_profile" "this" {
  for_each                = var.fargate_profiles
  cluster_name            = module.eks.cluster_name
  fargate_profile_name    = each.key
  pod_execution_role_arn  = aws_iam_role.fgp_exec[each.key].arn
  subnet_ids              = data.aws_subnets.private.ids
  # ... # other params

  provisioner "local-exec" {
    command = "aws eks wait fargate-profile-active --cluster-name ${self.cluster_name} --fargate-profile-name ${self.fargate_profile_name} --region ${self.region}"
  }
}

```

## My TF

See some of my TF usage at https://github.com/robinhutty/tf-states, https://github.com/robinhutty/tf, and https://github.com/robinhutty/tf-modules.
