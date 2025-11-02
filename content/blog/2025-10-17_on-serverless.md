---
title: "On Serverless Computing: the right tool for what kinds of tasks?"
date: 2025-10-17
modified: 2025-10-17T13:16:24
draft: true
type: blog
---

One of the topics that didn't come up in the (AWS) Serverless training and related at-work discussion that I've been around recently is:

"What kinds of tasks are/aren't suitable for Serverless?"

It seems that to the extent that the chatter about Serverless addresses disadvantages at all, it's mostly about:

* Vendor lock-in: not super relevant to us/many, many are already an all-in AWS/GCP/Azure/etc. shop
* Cold-start latency/performance: could be relevant, but there's typically a straightforward way to mitigate so even if the mitigation is "spend more", it becomes a business/FinOps issue not an architectural/technical issue
* Security risks: again, this matters and the details of achieving "sufficiently secure" systems will be different, but it doesn't seem that the effort required is markedly more (or less) than more traditional "serverful" computing architectures.
