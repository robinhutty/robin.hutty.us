---
title: On Code Review Semantics
type: docs
draft: false
---

Nowadays, it seems that Software Engineering, as a field of practice, has come to a consensus that Code Review is generally A Good Idea[^objection] - when a changeset is proposed as a Merge Request[^MR], someone/something SHOULD review before it gets merged. However, the semantics of code review have nuance that is context-sensitive - dependent on such situational aspects as: the kind of organization/team and the individuals, the overall purpose of the codebase, the purpose of the changeset, etc.

As is so common, "shared expectations" are critical to successful communication, so I've gathered some of my thoughts about the semantics of Code Review.

## Semantics

What does/should it mean to add a "LGTM"[^lgtm] to an MR? Well, that depends on the nature of the organization and the codebase. Here are some common semantics for approving a Merge Request[^multiple_reviews]:

* Review for unintended consequences; the claim is "To the best of my understanding, merging this changeset will not break anything".
* Review for the stated intent; the claim is "To the best of my understanding, merging this changeset will achieve the goal [of the ticket]"
* Review for programming language practice; the claim is "This changeset conforms to the project's coding guidelines about language/style/etc. and does not contain any unwarranted [code smells](../../glossary/#programming)".
* Code Review should generally _not_ be about whether the approver accepts the _intention_ of the changeset; generally, that consideration should have been decided in the Issue/Ticket that the changeset is intended to implement. In the case of projects that do not have ticketing/issue tracking, Code Review processes and the ensuing commentary for an MR has to serve that function as well - but I would argue that ticketing is appropriate for almost all codebases except possibly the most trivial.

[^multiple_reviews]: Often, a review will be for more than one of these purposes, but it's also common that different kinds of review can/should be done by different individuals.

## Who should review a Merge Request?

* The CODEOWNERS[^codeowners] convention uses a file that lists the expected reviewers for an MR based on the filenames in the changeset. Typically only used in software projects where there are many contributors, it's a useful convention to indicate individuals or groups of individuals who might be expected to know about each part of the codebase and therefore in a good position to review changes to that part.
* But also there is often value for a changeset to be reviewed by those are _not_ experts; reading code (in context) is immensely (and, sadly, more or less immeasurably) helpful in the quest to (a) gain familiarity with a codebase (b) develop expertise, such as with the technologies involved and/or the problem domain.

### Automated Review

* Many languages/formats/etc. have linting tools that can/should be used to help support a project's coding guidelines.
* LLM-based tools: with the rise of LLM-based AI, it's becoming commonplace to have such bots comment on MRs.

[^codeowners]: See documentation for [GitHub CODEOWNERS](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners) and/or [GitLab CODEOWNERS](https://docs.gitlab.com/user/project/codeowners/)

[^objection]: Some organizations, especially those which do not consider themselves to be "in tech", sometimes have projects which are "too small/trivial" to warrant code review and tooling/automation to support it as a standard practice. My arguments against this position start along the lines of: "Anyone can make a mistake" and "Additional set(s) of eyes help surface both problems and potential improvements". A slightly more sophisticated argument is basically: "We will want to add to/modify this codebase and it will be much easier to do so if changesets are reviewed before merge. Review improves Quality. Quality improves Velocity".


[^MR]: A Merge Request is a request to merge a changeset into an existing branch of a codebase. In the GitHub ecosystem and thereby much of the tech industry as a result of GitHub's dominance, the concept is referred to as a "Pull Request". I prefer the term "Merge Request", perhaps because it emphasizes that the codebase is not just _collecting_ the changeset, but also _incorporating_ it. But "Merge Request" and "Pull Request" are more or less synonymous.
[^lgtm]: "Looks Good To Me", often further abbreviated to ":+1:" or 👍.
