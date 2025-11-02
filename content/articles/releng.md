---
title: Release Engineering for Platform/Infrastructure
date: 2025-03-11
categories: engineering
tags: cicd, IaC
type: docs
draft: true
---

## About Compatibility and Breaking Changes[^cite-gitlab/glab]

1. We SHOULD follow the [SemVer][semver] specification.
1. We SHOULD NOT introduce breaking changes except when releasing a new MAJOR version. Unfortunately, there are situations where this is not possible, and we may introduce a breaking change in a MINOR or PATCH version. Some of situations where we may do so:

	* If a security issue is discovered, and the solution requires a breaking change, we may introduce such a change to resolve the issue and protect our users.
	* If a feature was not working as intended, and the bug fix requires a breaking change, the bug fix may be introduced to ensure the functionality works as intended.
	* When feature behavior is overwhelmingly confusing due to a vague specification on how it should work. In such cases, we may refine the specification to remove the ambiguity, and introduce a breaking change that aligns with the refined specification.
	* Experimental features are not guaranteed to be stable, and can be modified or removed without a breaking change.

	Breaking changes are a last resort, and we try our best to only introduce them when absolutely necessary.

[^cite-gitlab/glab]: Hat-tip to [gitlab-org/cli](https://gitlab.com/gitlab-org/cli/-/tree/v1.67.0?ref_type=tags#contributing) (aka the gitlab CLI `glab`) as influence.
[semver]: https://github.com/semver/semver/blob/v2.0.0/semver.md

TODO: write an intro and other material that talks about VCS, how/why platform and infrastructure engineering is a critical part of providing a software product, how P/I differs from the Application, etc.

Typically, for an organization that provides even a moderately sophisticated SaaS product, there will be several environments; some or all of: Production, UAT, SE, SRE, and likely a pile of development environments.

I'm using "Release Engineering" in this context to refer to the processes (and the tools that implement these processes) that manage how different versions of the code get deployed to each of these Environments.

That our version control systems(VCS), such as git, are a critical part of the tooling is a given - but how should we _use_ this incredibly powerful tool to help manage the complexity and achieve a reliable and comprehensible process that puts what we want where we want it? There are about a zillion ways to do this, most of them Obviously Wrong, many of them Wrong but not obviously so, and many of them with various advantages and disadvantages which are difficult to evaluate.

Below is one possible approach that provides rules/guidelines designed to make the releng process comprehensible, reproducible, TODO.
TODO: What qualities make for a good/better releng process?
* Not so onerous that engineers (or [product] management, etc.) need/want to use workarounds to achieve their goals.


## Guidelines

. The `main` branch MUST always pass its tests and the suite of tests SHOULD be developed so that it tends towards "prevents previously-discovered failure modes, prevents (some) undiscovered failure modes".
. The `main` branch SHOULD be the merge target of feature work.
. "Cutting a release" means committing a (git) tag and then running a [set of] deployment(s) that target Environment(s) using that tagged version of the codebase.
. (git) tags for releases MUST use [semver](https://semver.org/)
. There SHOULD be a record with: for each environment, which tag was used for the last deployment? Who/when was the last (successful/unsuccessful) deployment? With links to the monitoring/metrics dashboard, etc.
. The primary/shared development Environment SHOULD get a new deployment every time there is a merge to `main`.

## Scenarios

## Assumptions


## TBD

. Should Production deployments be curated in some way? Sequential (by Region?) or blue/green or canary or? What about pre-prod such as UAT/SE Envs?
