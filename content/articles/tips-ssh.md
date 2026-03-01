---
title: "Tips: SSH"
date: 2026-01-31
modified: 2026-02-17T12:46:58
categories: infrastructure
tags: ssh, tips
type: docs
draft: false
sidebar:
  open: true
---

SSH syntax/techniques that I have sometimes forgotten.

### Provisioning a key

Copy an SSH key to `~/.ssh/authorized_keys` to use key-based login[^strict]:

```console
ssh-copy-id -i ~/.ssh/<private_key_file> <user>@<host>
```

[^strict]: For machines that frequently get rebuilt/re-IPed AND are on a sufficiently safe/private network, you might use `-o StrictHostKeyChecking=no` and/or remove prior SSH host keys from `~/.ssh/known_hosts`.

### SSH client configuration

`ssh_config(5)`

1. Log in to a machine behind a jumpbox:
    ```config
      # ~/.ssh/config
  	  # Jump Box, accessible from outside
  	  Host jumpbox
      	  Hostname jumpbox.example.com
      	  IdentityFile ~/.ssh/robinhutty-2025.ed25519

  	  # Host on the remote LAN reached via Jump Box
  	  Host rpi-mm
      	  HostName 192.168.1.2  # unless/until there is DNS resolution _on the Jump Box_ that can resolve the hostname
      	  ProxyCommand ssh -qW%h:%p jumpbox
      	  IdentityFile ~/.ssh/robinhutty-2025.ed25519
    ```

	* Both the ultimate destination host and the Jump Box must have my ssh public key installed in `~/.ssh/authorized_keys`
	* Then I can, from a machine outside the LAN, run a command on the destination machine, such as to show the uptime ("laptop" is the machine I'm typing on):
      ```
        laptop$ ssh argon-mm uptime
        14:55:32 up 11 days,  2:23,  3 users,  load average: 0.00, 0.00, 0.00
      ```

2. Create a tunnel that forwards traffic received by a port on `laptop` to a remote destination, via a bounce box:

    ```
      $ ssh -g -L 54321:destination.example.com:80 bounce.example.com
    ```

3. Want multiple connections to the same destination? Use `ControlMaster` and `ControlPath`. This is particularly nice for git-over-ssh traffic.

    ```config
    # ~/.ssh/config
    ControlPath ~/.ssh/cp-%r@%h:%p
    ControlMaster auto
    ControlPersist 1h
    ```

4. With `ProtocolKeepAlives=0`, `KeepAlive=no` and `ClientAliveInterval=0`, the network connection can go down for an indefinite period and successfully be resumed at any time.

### SSH server configuration

`sshd_config(5)`[^caps]

```config
# /etc/ssh/sshd_config
# "PasswordAuthentication no" enforces the use of SSH keys
PasswordAuthentication no
# "PermitRootLogin prohibit-password" means that root can login with an SSH key, but not a password. "no" is slightly better and probably the correct choice for the cattle-not-pets servers I manage on the Internet.
PermitRootLogin no
# Mostly, there's just no need for X11 in my world, so let's not?
X11Forwarding no
```

You can view the fully resolved SSH server configuration as parsed by `sshd`, including defaults and any included files, which by default starts with `/etc/ssh/sshd_config`:

```console
$ sshd -T | sort
port 22
addressfamily any
listenaddress [::]:22
listenaddress 0.0.0.0:22
usepam yes
# <snipped>
```

[^caps]: For human legibility, configuration files typically use medial capitals (aka CamelCase) for keywords, but the OpenSSH manual pages specify: "keywords are case-insensitive and arguments are case-sensitive".

### See Also

* my article on [ssh for (multiple) vcs/forges](..//vcs-identities/#sshconfig)
* To cope with poor/intermittent connectivity, consider [mosh](https://mosh.org/).
* https://en.wikibooks.org/wiki/OpenSSH

### Reference Documentation

* [OpenSSH Manual][1]
* [man ssh(1), the client][2]
* [man ssh_config(5), the client configuration file][3]
* [man sshd(8), the server][4]
* [man sshd_config(5), the server configuration file][5]

[1]: https://www.openssh.org/manual.html
[2]: https://man.openbsd.org/ssh
[3]: https://man.openbsd.org/ssh_config
[4]: https://man.openbsd.org/sshd
[5]: https://man.openbsd.org/sshd_config
