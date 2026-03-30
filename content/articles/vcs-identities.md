---
title: "VCS: How I use multiple git identities and forges"
date: 2025-11-25
modified: 2026-03-21T22:03:04
type: docs
categories: devx, VCS
tags: vcs, devx
draft: false
---

## Multiple identities (including at a single software forge)

When an employer or other organization wants me to work with one of the same software forges that use elsewhere in my life, I use a different identity. How do I keep this cleanly configured so that I git with the correct identity and that It Just Works (once I've set it up)?

* Per-organization local directories
* A different SSH key and some per-organization configuration for `~/.ssh/config`.
* A different email address/commit signing key.

So for "Org1" with "me@org1.example.com" and "Org2" with "me@org2.example.com"

### `~/.ssh/config`[^ssh]

```config
# ~/.ssh/config
Host github.com-me
    HostName github.com
    User git
    IdentityFile ~/.ssh/rhu-2026-ed25519

Host github.com-org1
    HostName github.com
    User git
    IdentityFile ~/.ssh/rhu-org1-2026-ed25519

Host myforge, vcs
    # HostName might be different depending on LAN DNS?
    HostName forge.example.com
    User git
    IdentityFile ~/.ssh/rhu-2026-ed25519
```

### `~/.gitconfig-org1`

```config
# ~/.gitconfig-org1
[user]
  name  = My Name
  email = me@org1.example.com
  signingKey =  ~/.ssh/rhu-org1-2026-ed25519
```

### `~/.gitconfig-org2`

```config
# ~/.gitconfig-org2
[user]
  name  = My Name
  email = me@org2.example.com
  signingKey =  ~/.ssh/rhu-org2-2026-ed25519
```

### `~/.gitconfig`

```config
# ~/.gitconfig
### <snip>
[commit]
  gpgSign = true
[gpg]
  format = ssh
[user]
  name  = My Name
  email = me@personal.example.com
  signingKey =  ~/.ssh/rhu-2026-ed25519
[includeIf "gitdir:~/orgs/Org1/"]
  path = ~/.gitconfig-org1
[includeIf "gitdir:~/orgs/Org2/"]
  path = ~/.gitconfig-org2
### <snip>
```

### Create directory structure and ssh keys

```console
$ mkdir -p ~/orgs/{org1,org2}
$ git clone git@github.com-org1.com:org1/repo1.git ~/orgs/org1/repo1
```


TODO: coping with multiple repositories: manyrepos, gickup, etc.

TODO: See Also/hat-tip to https://flori.dev/reads/git-signing-and-multiple-identities/ and https://medium.com/@leroyleowdev/one-machine-many-identities-adding-effortlessly-switch-between-multiple-git-profiles-fd56a20bc181 and https://dev.to/victorbruce/managing-multiple-git-identities-a-seamless-workflow-for-personal-and-work-accounts-1kce

TODO: about ssh-signing: https://codon.org.uk/~mjg59/blog/p/ssh-certificates-and-git-signing/

[^forge]: a [software forge](https://en.wikipedia.org/wiki/Forge_(software)), a collaboration service/platform for software development.
[^ssh]: See my [SSH tips article](../tips-ssh/)
