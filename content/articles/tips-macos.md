---
title: "Tips: macos"
date: 2026-02-05
modified: 2026-02-11T17:40:33
categories: productivity
tags: macos, tips
type: docs
draft: true
---

I've been using OSX/macos as a daily driver workstation/notebook device[^mactop] for many years, originally because it was a unixlike OS that worked well [enough] on good [enough] hardware with good support for that hardware[^leave].

## System

* [Homebrew](https://brew.sh) makes macos acceptable. (I also use many machines with [Homebrew on linux][3])
* See my [article](../macos-config/) about my [macos configuration management](https://github.com/robinhutty/ansible-collection/)
* Adjust VRAM Allocation on Apple Silicon Macs (verified on Sequoia/Tahoe): `sudo sysctl iogpu.wired_limit_mb=(size in MB)`. (This could help LLama and gaming, but comes with stability risks).

### Keyboard-driven life

* [Vimium][4] (Extension/Add-on) for browsers based on Chrome and Firefox.
* One of the tiling window manager projects: [glide][2]?

[2]: https://glidewm.org/
[3]: https://docs.brew.sh/Homebrew-on-Linux
[4]: https://github.com/philc/vimium

### Version Specific

#### MacOS 26 (Tahoe)

* Liquid Glass: System Settings > Accessibility > Display > Reduce Transparency to ON
* Animation: System Settings > Accessibility > Motion > Reduce Motion to ON

## Networking

1. After modifying `/etc/hosts`, flush the DNS cache:

	```console
	$ sudo dscacheutil -flushcache; sudo killall -HUP mDNSResponder
	```

1. neo-network-utility

	The OSX Network Utility has been missing since Catalina, neo-network-utility is pretty similar for Sequoia/Tahoe.

	```console
	$ brew install neo-network-utility
	```

## Virtualization

* vz, lima, colima

## Other

* LM Studio

[^leave]: Unfortunately for me, nowadays (and for many years) AAPL seems pretty keen on making changes that I don't care about or even actively de-prefer. But inertia/lock-in and spoon allocation are keeping me on macos.
[^mactop]: I also use linux machines as daily drivers. Yes, I'm a nerd who has several different machines that I use daily - who is surprised?
