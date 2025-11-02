---
title: Dates, Deliverables, and Estimation
date: 2025-08-28
modified: 2025-10-29T11:17:38
type: docs
draft: true
category:
tags:
---

"Dates are a real thing in our business. You have made it clear that you have a philosophical aversion to aiming for a date for a specific deliverable on the basis that in software development the unexpected can always happen. This is completely true, but in no way creates an excuse for not naming dates and aiming for them. The approach we will follow is where we have dates and deadlines and we work towards them and if something material transpires that means one of those dates is missed or needs to get adjusted, then we address that." - VP Engineering

The social contract here is that I am given the opportunity to succeed, that the company/its staff either works in a way that supports my success or accepts that the success it wants from me is necessarily limited. Anything else is pure fantasy and wishful thinking.

I understand and accept that they/the business needs to know when things are going to happen, but we need to set some expectations about what is reasonable for me/my team in terms of date/time estimation:
1.	Estimation _itself_ takes time. Sometimes significant research is required, sometimes there is a dependency on some other internal or even external team, which means that we cannot reasonably make the estimate itself at the time of the initial request. This is further exaggerated if, as is the case with this team, (a) there's only one person who is involved with the estimation effort (b) there are many projects in progress (c) there is a huge backlog
2.	Estimates can only meaningfully comment about how long something will take _once I can give it my attention_ and I'm not in control of that; someone else is always capable of demanding my attention be put to whatever is highest on their list.
3.	Estimation does not work when I discover that the request is substantively incomplete/incorrect. The request said "The Export API will talk to a Pathway internal API over a VPC Endpoint" which I took to mean that this is barely different from when we added support for KISS or AWF. But the reality is that there is much more involved such as a new operator for the K8s cluster, new networking, some IAM work, coordination/consultation with ACE, and I don't even yet know what else)
4.	If I discover (again) that there is more involved than the above suggests, then the estimate will need to be extended, but I will not know until after it bubbles up to be my top priority and I get to start on it.
5.	As always there are also assumptions about the amount of disruption: emergency/urgent operational efforts (which will almost always take priority over any work to support/release something new), team illness/PTO/etc., other projects being given higher priority, etc. These assumptions have an especially powerful impact for a tiny team (compared to the number of individuals/teams who rely upon us) because we have no room for surprises.
6.	If the project being estimated is #10 on my team's priority list, the uncertainty about when it gets initial attention is very high AND relevant details are likely to have changed so as to make any estimate of duration extremely speculative.
