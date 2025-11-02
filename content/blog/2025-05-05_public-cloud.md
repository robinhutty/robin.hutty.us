---
title: Self-Hosting in Public Cloud
date: 2025-05-05
type: blog
---

Sometimes "Self-Hosting" means or implies "on your own hardware, in your own home or a co-location centre". But sometimes it means "on a Virtual Machine provisioned by a public cloud vendor".

For most of my self-hosted services, I have little need for Serious Uptime, so I can just use whatever old/puny hardware that I have lying around the house and my ISP's connectivity (now via fibre, not satellite!) is Good Enough. And if the Raspberry Pi (Pi4B with 8G RAM) can cope, the marginal cost is tiny: the [datasheet](https://datasheets.raspberrypi.com/rpi4/raspberry-pi-4-datasheet.pdf) says 15W max.

But there are some few services that are important enough to me that I'd like them to be on hardware that is maintained by someone else, with redundant power/networking. Fortunately, for those latter cases, the compute/network/storage needs are low, and nowadays it can often be very inexpensive[^iaas-costs].

## What do I want to run in Public Cloud/IaaS?

Not Much[^mx]

* Authoritative DNS for a small number of Zones that do not see heavy traffic.
* Dynamic DNS service so that in the (unusual) event that my home IP address changes, services at home can still be reached by name.
* Off-site backup for some personal VCS repositories.

## How?

My career has largely been using Infrastructure as Code and Configuration Management to do exactly this sort of thing, so this is pretty straightforward for me and all I have to do is remember that the decision-making process is different: I don't have SLIs to maximize or SLAs to meet so I should only (over-)engineer as much as delights me.

[^iaas-costs]: I can provision a VM on AWS or Digital Ocean for less than $10 per month. It will be pretty puny (1-2vCPUs, 0.5-1GiB RAM), but powerful enough for my purposes.
[^mx]: I have given up self-hosting email services after a couple of decades, mostly because the major megacorp Email providers (Google, Microsoft, and a few others) make it so difficult to achieve consistently reliable deliverability, even if you're not compromised/spamming.
