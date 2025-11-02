---
title: "Technical Decision Making as part of Engineering Management"
date: 2025-06-20
modified: 2025-09-26T14:44:37
categories: engineering
tags:
type: docs
draft: true
---

NOTE: This is for Engineering projects once a decision has been made to Build versus Buy[^bvb].

[^bvb]: Build vs. Buy is an important topic/decision in its own right; see [some other discussion](https://entropicthoughts.com/build-vs-buy), Greenspun’s tenth rule: "Any sufficiently complicated C or Fortran program contains an ad hoc, informally-specified, bug-ridden, slow implementation of half of Common Lisp."

NOTE: I do not pretend to be a scholar of Decision Science or Cognition, or indeed an academic of any kind.

TODO: Mention ADRs

Three kinds of goal-oriented activity:

0. Start with at least one goal (and possibly some non-goals).
1. Strategy: identifying a high-level approach.
2. Operations: whether and how to prepare to execute the strategy.
3. Tactics: doing the work.

## Context

For years now, I have had "Principal" in my job title, and despite what I think about title inflation, I have been the most senior technical Individual Contributor[^IC] on all my primary work projects, so I suppose it's technically correct.

This has two major consequences:

* I don't get to say "I don't know why this is isn't working/how to fix it"
* I'm responsible for making technical decisions about the Platform, Infrastructure, Security, etc. such that the goals can be met and the constraints can be satisfied

This article is a general piece concerned with the latter:

1. How do/should I approach these decisions?
2. What factors should I consider?

And it's probably going to be rambly mess, possibly not forming a coherent piece, even to me, because writing is linear and thought is complex; but it might give the reader[^me] some useful prompts.

## Different Categories of Decisions

* Should we do _foo_? (or not)

Breaking even on benefits with the existing approach isn't enough to switch, the new approach needs to have enough quantitative benefit to
Be worth the total engineering time to make the switch
Be worth the risk of being in a split state for an extended period if something comes up (so now there's pattern confusion)
Less important, but be worth the friction for the number of people impacted by the change who have to relearn how to do something they can already do
It's a very different conversation if we're talking "Existing library / pattern has been deprecated, is no longer supported, or is considered an anti-pattern for security" vs "There's a newer, lighter weight framework that does the same thing"


[^IC]: Like many, but not all, I use "Individual Contributor" to mean "those who do not have managerial responsibility for other employees (even if they do have responsibilities for engineering management/project management)".
[^me]: The [main](../../about/#about-this-sites-content) target audience for this whole website is _me_.

## See Also

[fence]: https://fs.blog/chestertons-fence/

----

References:

> Thank you for your input. Bold people, willing to speak and add color, make the meeting more than a lecture; it creates a conversation. Really appreciate your voice.

> keep speaking up... keep honing the process. Your voice not only matters, but is needed and appreciated.

Mike Reed, in response to me sharing concerns about [Architecture Review Board] process and how to mitigate them.

 In the final planning meeting before submitting the bid to the customer, the project manager set forth an incredibly aggressive and unachievable schedule to be given to the customer. I objected forcefully in the meeting — after all, we didn’t even have an architecture yet, much less a design, yet the project manager already had a fixed completion date — and later that afternoon, I wrote up a memo listing thirteen (13) major risks I saw to the project. While some of the engineers on the project cheered the memo, management told me in so many words to shut up and architect.

From [someone I don't know](https://brucefwebster.com/2008/04/15/the-wetware-crisis-the-themocline-of-truth/):
>
> However, less that two months later, I wrote a new memo — based on the old one — and pointed out that 12 of the 13 risks I had pointed out had actually come to pass. Shortly after that, the project manager had to go back to the customer with a new delivery schedule that was twice as long as the original one. A month or two after that, my role as an architect came to an end. I had a final lunch with the two head honchos in upper management, and to their credit, they asked for my final assessment. I told them that many of the bumps and potholes were just part of the software development process — but that they should never have given that blatantly unrealistic schedule to the customer. As I told them, “When you do something like that, in the end you look either dishonest or incompetent or both. And there’s no upside to that.”
>
> A few months after I left, I got word that the schedule had slipped to three times the original length, and not long after that, I got word that the customer had canceled the project altogether. As I said: just no upside.
