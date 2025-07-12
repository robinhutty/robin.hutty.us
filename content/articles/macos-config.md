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

* Boot the machine, creating a user account/password.
* Optional networking: give the LAN's DHCP server the new MAC address(es) so the new machine gets configuration for hostname/IP, DNS.
* Run system updates, if not already completed.

### SSH

* Open "System Settings", select "General > Sharing". Under "Advanced", enable "Remote Login".
* From another machine use 'ssh-copy-id' or otherwise add your SSH Public Key.
* Consider distributing the SSH Host Key to other machines so that I can use `StrictHostKeyChecking=true`.

### Homebrew

* "The Missing Package Manager" - https://brew.sh
* `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"`
* Use a `Brewfile` (via `brew bundle`) to install many of my favourite software packages.

### Modifications to the default Security posture

* Open "System Settings", select "Privacy & Security". Select "Full Disk Access" and add/enable "iTerm"[^bigsur]. Because I live in a terminal-based world and I want to be able to use the files on my filesystem. This one came up because I downloaded an HTML file (using a gui web browser) and macos used Extended Attribute functionality keep me from hurting myself.

### Set default applications for various filetypes

I use [infat](https://github.com/philocalyst/infat): "A command line tool to set default openers for file formats and url schemes on macos".

* Have your `~/.zshrc` (or whatever) set `$XDG_CONFIG_HOME`
* You can get `infat` to initialize a configuration with the currently set associations with `infat --config ${XDG_CONFIG_HOME}/.infat/config.toml init`
* Configure `infat` which will, by default, read from `$XDG_CONFIG_HOME/infat/config.toml`:

```toml
[types]
video = "VLC"

[schemes]
mailto = "Thunderbird"
web    = "Google Chrome"

[extensions]
html  = "Google Chrome"
pdf   = "Skim"
epub = "Koodo Reader"
```


### Maybe?

* Opinions about ["System Integrity Protection"](SIP)[^capitan] are somewhat varied and while I have looked into this occasionally, I don't currently remember enough to have an opinion that is well-enough considered or informed that I'm willing to expound on the web about it.

### Configuration Management

Add the new machine to (Ansible) inventory/group(s) and run a Playbook that applies at least the `common` Role, which will install various software packages that I want, dotfiles, etc.


## References

[SIP]: https://support.apple.com/en-us/HT204899


[^automation]: At the moment, I'm using [Ansible](https://docs.ansible.com/) for this kind of automation; progress can be see at TODO (VCS repo link).
[^sequoia]: This is for Mac OS Sequoia (15.x), although much of it should be applicable to (a few?) older and newer releases.
[^bigsur]: This behaviour started with Mac OSX Big Sur (11.x).
[^capitan]: This behaviour started with Mac OSX El Capitan (10.11.x).
