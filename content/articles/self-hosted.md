---
title: Self-hosted services
date: 2025-04-01
modified: 2025-09-27T12:57:37
categories: self-hosted
tags: cicd, IaC
type: docs
draft: true
sidebar:
  open: true
---

## User facing

{{% details title="Communication" closed="true" %}}

### Email

* https://stalw.art/mail-server
* https://github.com/knadh/listmonk

### Chat

* Matrix/Conduit?
* https://github.com/revoltchat/self-hosted ?
* XMPP: https://prosody.im, http://coccinella.im

### Video

* Jitsi-Meet: https://jitsi.github.io/handbook/docs/devops-guide

{{% /details %}}

{{% details title="Media Services" closed="true" %}}

* https://github.com/janeczku/calibre-web
* https://www.audiobookshelf.org/
* https://dtcooper.github.io/raspotify

## HTTP

* Caddy:
* Analytics: umami
* RSS: https://miniflux.app/docs/index.html
* Search: https://github.com/searxng/searxng
* Personal dashboard: https://github.com/phntxx/minidash, https://github.com/jeroenpardon/sui

{{% /details %}}

{{% details title="Networking/LAN" closed="true" %}}

### IP

- tunnel to retain dynamic IP: autossh via systemd. See the [SWiP tunnel docs](https://gitlab.com/some-work-in-progress/tech/-/blob/main/tools/systemd_services/tunnels.adoc)
- IPAM?

### DHCP & DNS

- pihole
- Dynamic DNS client to record the [LAN's public IP](). See the [SWiP ddns docs](https://gitlab.com/some-work-in-progress/tech/-/blob/main/tools/systemd_services/ddns.adoc)

{{% /details %}}

## Developer Services

{{% details title="Identity and Access Management(IAM)" closed="true" %}}
* [Authentik](https://goauthentik.io/)
{{% /details %}}

{{% details title="VCS/Code Forge" closed="true" %}}

VCS, CI/CD, etc.

* https://forgejo.org
* Mirror repositories with [gickup](https://github.com/cooperspencer/gickup)

{{% /details %}}

{{% details title="Metrics, Monitoring, Alerting, and Observability(MMAO)" closed="true" %}}

* Push-based notification: https://ntfy.sh
* https://healthchecks.io

{{% /details %}}

{{% details title="Mirrors/Archives" closed="true" %}}
* homebrew mirror
* debian mirror
{{% /details %}}
