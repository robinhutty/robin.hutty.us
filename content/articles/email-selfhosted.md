---
title: Self-Hosted Email Services with Stalwart
date: 2025-05-04
categories: self-hosted
tags: email
type: docs
draft: true
---

Email has some important qualities:

* durable via archives
* accessible
* federated/decentralized
* Open, non-proprietary, and not dependent upon a specific company or set of MegaCorps[^mega]

[^mega] This is still just about true, despite a long history of efforts by several Big Tech companies to create walled gardens (especially AOL, Hotmail/MSFT, Yahoo, GMail/Google), mostly in the name of spam prevention, but with such a strong effect to make it difficult for small orgs/individuals to control their own mail that it's difficult to believe that this was not a deliberate consequence despite [Hanlon's Razor](https://en.wikipedia.org/wiki/Hanlon%27s_razor).

## Intended Components

- [Stalwart](https://stalw.art)
- [Storage backend](https://stalw.art/docs/get-started#choosing-a-storage-backend):
  - Data store: SQLite/RocksDB/FoundationDB?
  - Blob store: filesystem|s3/minio?
  - Full-text search store: SQLite/RocksDB/FoundationDB? (ElasticSearch for large or busy installations)
  - In-memory store: SQLite/RocksDB/FoundationDB? (only necessary to bother with (Redis|memcached) for large or busy installations)
- [Authentication backend](https://stalw.art/docs/get-started#choosing-an-authentication-backend): [Authentik](https://goauthentik.io/)
- Reverse Proxy: [Caddy](https://caddyserver.com)



## Controversy

In 2025, it is somewhat controversial to talk about self-hosting small-scale (e.g. friends/family) email services; the reasoning offered tends to fall into three main categories:
1. The benefit is often considered minor because there are so many cheap/free options available: gmail, MSFT?, etc.
2. It is considered difficult/time-consuming to setup and manage
3. Deliverability is a difficult problem because most of the world's users use one of a very small number of email providers, most of which penalize small service providers in the name of spam prevention whether or not they are sources of spam

### Response

1. One can each decide whether the benefits are worth it or not; for example, when I originally ran my own personal mail services (I think sometime in 2004 or 2005), I embarked on the process mostly for educational reasons because I was new(-ish) to a techie career. With hindsight, I still consider it well-spent time/effort.
2. Nah, not really. There's a moderate amount of initial effort to set it things up and tweak to best meet the use case, but the ongoing effort (over about twenty years) was a handful of hours per year on average.
3. Deliverability is a Real Problem - and the reason I gave up running mail services for my main personal email domain.
