---
title: Glossary
date: 2025-07-16
modified: 2025-09-27T10:39:20
type: docs
---

This website uses _many_ technical terms, some of which have multiple usages or uncertain semantics - this list attempts to collect some definitions for the way some of them are used on this site.

Platform
: a technology or set of technologies that is used as a destination to run application or infrastructure services; e.g. container orchestration platforms such as Kubernetes/Docker/etc. => containerized applications, bare-metal => physical machines, hypervisor => virtual machines, etc.

Infrastructure
: typically used in a fairly generic sense; usually to contrast with "the Application(s)"

Everything As Code
: a derivative of the phrase "Infrastructure As Code(IaC)", this refers to the practice of treating everything that is required to build/run the whole thing with the same respect as is given to the code that is the applications themselves. TODO: such as?

Environment
: an execution environment for an individual Process, typically referring to a posix[-like] and the Environment Variables, OS API, etc. that are available to the Process.

Feature Work
: engineering new or expanded functionality, cf. chore, bugfix, testing, documentation, (code)style, etc.

Chore
:

Continuous Integration
: integrating source code changes frequently, intended to help ensure that the codebase is always in a workable state ("Don't break the Build"). Typically includes various kinds of automated testing.

Continuous Delivery/Deployment
: often used synonomously (and in a single phrase with Continuous Integration styled as "CI/CD"), these terms refer to (automated) processes to get the software product/service to a/the destination whether that is an environment for further testing or to "Live in Production".

Semantic Versioning
: A conventional approach(`<Major>.<Minor>.<Patch>`) to identifiers for a Release to indicate information about compatability/breaking changes. See [semver.org](https://semver.org/) for details.

Diátaxis (documentation)
: The [Diátaxis](https://diataxis.fr/) framework categorizes documentation by "audience purpose": how-to, explanatory, tutorial, reference. Discussion [here](https://idratherbewriting.com/blog/what-is-diataxis-documentation-framework) and on [HN](https://news.ycombinator.com/item?id=42325011)

Toil
:

Role-Based Access Control
:

## Networking

Public IP
: The unique Internet Protocol address that refers to your LAN or home network from the perspective of the WAN, the Internet, "the outside".

Private IP
: The unique Internet Protocol address that refers to an individual device on your LAN or home network from the perspective of the LAN, "the inside".

NAT (Network Address Translation)
: Typically, this is how the router that manages a LAN can send response traffic back from a server on the public Internet to the device/client inside the LAN that initially requested it.

Port forwarding
: when a router or firewall receives traffic (IP packets) that are _not_ responses, the only way it knows where to send them is via port forwarding rules.

LAN (Local Area Network)
: the inside, typically a home/office network.

WAN (Wide Area Network)
: the outside, typically the public Internet.

DNS (Domain Name System)
: at a fundamental level, the DNS converts hostnames to IP addresses. There a several different kinds of records in common use - the most common of which NS, A, AAAA, CNAME, TXT, MX.

## Software Development/Programming

"code smell"
: If some code or changeset seems [likely] to _work_, but appears to be problematic in some way, it is said to have "code smell(s)"; typically this refers to characteristics that likely reduce quality on some axis, such as one or more of: security, reliability, maintainability/extensibility, testability, performance, etc. Start with [Martin Fowler](https://martinfowler.com/bliki/CodeSmell.html) to discover history, nuance, and detail.

CODEOWNERS
: A file in a VCS repository that indicates the expected reviewers for a MR based on the files in the changeset

linter
: a tool that performs static code analysis to help surface bugs, stylistic errors, and suspicious constructions/patterns

dynamic analysis
: analysis of computer programs performed by executing them

static analysis
: analysis of computer programs performed _without_ executing them

(software) forge
: a service/platform for collaborative software development; they provide VCS repositories and typically many other features: code review, CI/CD, artifact repository/release management, issue tracking/project management and more. Examples of services/software include: github.com, gitlab.com, codeberg.org/forgejo.org, sourcehut.

Pull Request/Merge Request
: a request to incorporate a proposed changeset into a (typically shared) codebase.

## Security

DAST
: Dynamic Application Security Testing

SAST
: Static Application Security Testing

## Design

Typeface
:

Font
:

## Other Resources I like

These mostly provide information aimed to increase understanding (and not so much tutorial or reference docs)

* [Beej's Guide to Network Concepts](https://beej.us/guide/bgnet0)[^bgnet]

[^bgnet]: not to be confused with the venerable and also awesome [Beej's Guide to Network Programming](https://beej.us/guide/bgnet/), which aims to be a practical tutorial for programming using (TCP/IP) network sockets in C.
