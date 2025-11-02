---
title: VMs on macos for testing
type: docs
draft: true
sidebar:
  open: true
---

If/when I want to test (server) installation and other other automation, it's convenient to use Virtual Machines. And since I'm largely using macos for machines that I use directly[^typing], let's not deal with `libvirt` or anything just yet.

## [UTM.app][utm]

UTM
: "UTM is a full featured system emulator and virtual machine host for iOS and macOS. It is based off of QEMU[^appl-virt]."

As is my wont, let's script it some. [UTM uses AppleScript][3]:

```applescript
```

[utm]: https://docs.getutm.app
[2]: https://developer.apple.com/documentation/virtualization
[3]: https://docs.getutm.app/scripting

[^typing]: laptops/desktops that I type on _their_ keyboards, instead of accessing remotely via ssh.
[^appl-virt]: It can also use [Apple Virtualization][2], although this is still considered experimental at the time of writing.
