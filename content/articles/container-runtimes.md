---
title: "Containers: runtimes, tools, etc."
date: 2026-01-25
modified: 2026-02-28T16:33:10
categories: infrastructure
tags: container, devx
type: docs
draft: true
sidebar:
  open: true
---

This article is to collect some reminders about certain container technologies and techniques. It is not an introduction to containers/Docker.

## Installation and setup for containerd/nerdctl/rootlesskit

[NOTE: As of February 2026, [nerdctl](https://github.com/Homebrew/homebrew-core/commits/main/Formula/n/nerdctl.rb) and [rootlesskit](https://github.com/Homebrew/homebrew-core/pull/269826) are both available in [Homebrew on Linux](https://docs.brew.sh/Homebrew-on-Linux) for both amd64 and aarch64.]

> [containerd][3] is an industry-standard container runtime with an emphasis on simplicity, robustness, and portability.

> [nerdctl][6] is a Docker-compatible CLI for containerd [with [some additional features][7]].

* I want to use [CNI plugins][8] to provide networking options: bridge, portmap, firewall, tuning.
* I want to be able to run non-privileged containers via [rootless mode][5].
* Therefore, I use the 'nerdctl-full' package from [containerd/nerdctl releases](https://github.com/containerd/nerdctl/releases)

### Installation

On Armbian (based on Debian Trixie):

```console
robin@localhost:~$ sudo apt install -y uidmap
robin@localhost:~$ NERDCTL_VERSION=$(curl -s https://api.github.com/repos/containerd/nerdctl/releases/latest | grep tag_name | cut -d '"' -f 4 | sed 's/v//')
robin@localhost:~$ wget https://github.com/containerd/nerdctl/releases/download/v${NERDCTL_VERSION}/nerdctl-full-${NERDCTL_VERSION}-linux-arm64.tar.gz
robin@localhost:~$ sudo tar -xzvf nerdctl-full-${NERDCTL_VERSION}-linux-arm64.tar.gz -C /usr/local/
```

```console
robin@localhost:~$ containerd-rootless-setuptool.sh install
<snip>
robin@localhost:~$ systemctl --user start  containerd.service
robin@localhost:~$ sudo loginctl enable-linger $(whoami)
robin@localhost:~$ nerdctl version
Client:
 Version:       v2.2.1
 OS/Arch:       linux/arm64
 Git commit:    0d1089396f017bb872ad40606b0d31ebdeaa828a
 buildctl:
  Version:      v0.26.3
  GitCommit:    c70e8e666f8f6ee3c0d83b20c338be5aedeaa97a

Server:
 containerd:
  Version:      v2.2.1
  GitCommit:    dea7da592f5d1d2b7755e3a161be07f43fad8f75
 runc:
  Version:      1.4.0
  GitCommit:    v1.4.0-0-g8bd78a9
```

#### bypass4netns

Use [bypass4netns][9] for much faster networking[^bypass].

```console
robin@localhost:~$ containerd-rootless-setuptool.sh install-bypass4netnsd
robin@localhost:~$ systemctl --user start  bypass4netnsd.service
```

Then I can add annotations to my containerized services, such as in the `compose.yaml`:

```yaml
services:
  foo:
    image: <some_image>
    annotations:
      bypass4netnsd.service: true
    ...
    ports:
      - "8080:80"
    ...
```

[^bypass]: Something like 8x compared to `slirp4netns`! See https://github.com/rootless-containers/bypass4netns/tree/0f2633f8c8022d39caacd94372855df401411ae2

## See Also

* [lima documentation][1]
* [colima documentation][2]

[1]: https://lima-vm.io/docs/
[2]: https://colima.run/docs/
[3]: https://github.com/containerd/containerd
[4]: https://github.com/containerd/containerd/blob/v2.2.1/docs/getting-started.md
[5]: https://github.com/containerd/nerdctl/blob/main/docs/rootless.md
[6]: https://github.com/containerd/nerdctl
[7]: https://github.com/containerd/nerdctl?tab=readme-ov-file#features-present-in-nerdctl-but-not-present-in-docker
[8]: https://github.com/containerd/nerdctl/blob/main/docs/cni.md
[9]: https://github.com/rootless-containers/bypass4netns
