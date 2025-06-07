---
title: "MacOS: Configuration"
type: docs
draft: false
---

# Runbook for a new workstation install

What is this?

* A description of what I _want_ to happen[^sequoia] as a record/todo-list before I get around to automating it all[^automation] (and to help me write that automation!).
* Only about "workstation" machines, and by that I mostly mean "laptop/desktop machines with keyboard and screen that I use directly - not servers".
* Only about MacOS machines, _not_ changes that I want to make to all/most machines that I use such as linux.

## Action!

* Boot the machine, creating a user account/password

### SSH

* Open "System Settings", select "General > Sharing". Under "Advanced", enable "Remote Login".
* From another machine use 'ssh-copy-id' or otherwise add your SSH Public Key.

### Homebrew ✅

* "The Missing Package Manager" - https://brew.sh
* `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
  * Use a `Brewfile` (via `brew bundle`) to install many of my favourite software packages.

### Modifications to the default Security posture

* Open "System Settings", select "Privacy & Security". Select "Full Disk Access" and add/enable "iTerm"[^bigsur]. Because I live in a terminal-based world and I want to be able to use the files on my filesystem. This one came up because I downloaded an HTML file (using a gui web browser) and macos used Extended Attribute functionality keep me from hurting myself.

#### Maybe?

* Opinions about ["System Integrity Protection"](SIP)[^capitan] are somewhat varied and while I have looked into this occasionally, I don't currently remember enough to have an opinion that is well-enough considered or informed that I'm willing to expound on the web about it.


## References

[SIP]: https://support.apple.com/en-us/HT204899


[^automation]: At the moment, I'm using [Ansible]() for this kind of automation; progress can be see at TODO (VCS repo link).
[^sequoia]: This is for Mac OS Sequoia (15.x), although much of it should be applicable to (a few?) older and newer releases.
[^bigsur]: This behaviour started with Mac OSX Big Sur (11.x).
[^capitan]: This behaviour started with Mac OSX El Capitan (10.11.x).
