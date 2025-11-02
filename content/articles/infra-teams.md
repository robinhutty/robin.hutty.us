---
title: On Infrastructure and Platform Engineering
date: 2025-05-08
modified: 2025-09-26T14:37:26
categories: engineering
type: docs
draft: true
---

I'd like to get more mindshare for the idea that these teams have _two_ products and that both are fundamental requirements for fulfilling the team mission:

1. The infrastructure, etc. resources (in the Cloud) that serve our stakeholder needs (paying customers plus the dev/product folks for the apps)
2. The codebase, technologies, and the processes that we use to design/build/operate the above product.

Platform is [a] Product.

Delivery Infrastructure / Software Delivery Enablement / Eng Services

Software development is about contracts.

Platform Engineering aims to [avoid the current pain/coming disaster] with a "contract" between application development teams and system owners.

Consistency vs. Independence: app teams wanna app however they want (or Product teams tell them to), but platform teams have to require at least a certain amount of consistency.

As long as teams use a specific class of tools, the particular tool matters less.

Some realities of software development:

* Application Developers vastly outnumber Infra/Platform/Security/SRE/etc. _combined_, usually many times over.
* Cognitive load is the enemy of those responsible for building/running complex systems
* Abstractions (including Contracts/Promises) are the standard approach to managing the complexity and therefore the cognitive burden on those who must build/run these systems.

Toil (as defined in the SRE book[^SRE])
: tends to be ... repetitive, automatable, tactical, devoid of enduring value, and that scales linearly as a service grows.

[^SRE]: [Site Reliability Engineering: How Google Runs Production Systems][sre-link]; Murphy, Beyer, Jones, & Petoff; O’Reilly Media; 2016.
[sre-link]: https://sre.google/sre-book/table-of-contents/
