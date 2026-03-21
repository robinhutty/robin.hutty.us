---
title: "Raspberry Pi4: Initial Configuration"
date: 2025-08-12
modified: 2026-03-21T18:54:12
categories: configuration management
tags: linux,pi,installation
type: docs
draft: true
---

I have a Raspberry Pi4 with a nice little [case][1] that I use as a bastion/jumpbox for a LAN. Upon installation/first boot of an [Armbian OS][2] based on [Debian 13(Trixie)][3], it prompts me to create a root password and a local user (who gets `sudo` privileges).

There are a couple of tiny changes that I make by hand (because it's sufficiently easy/infrequent/safe that it's not worth automating) before moving on to automated configuration management.

1. Hostname

    ```console
    $ sudo hostnamectl hostname <fqdn>
    ```

1. Network

	Armbian uses [Netplan.io][5] to describe networking configurations. Netplan provides great [howto guides][6]. So it's easy to configure my preferred networking for this kind of machine: a manual IP address for wired ethernet, custom nameservers.

## Configuration Management

From a control[^ansi-controller] machine (my laptop):

```console
$ ssh-copy-id -i ~/.ssh/robinhutty.ed25519 robin@rpi
```

[^ansi-controller]: An [Ansible control node](https://docs.ansible.com/projects/ansible/latest/getting_started/index.html)

My Ansible Collection has a special accommodation for this machine, which is to install/execute a custom shell script to make the system fan less aggressive, but otherwise this machine is treated as just another (arm64/aarch64) machine belonging to certain Ansible Groups. These group memberships dictate the software and configuration applied to this machine.

## See Also

* [Avoiding SD card wear, etc.][4]
* My Ansible that configures this as bastion box and some lite services using caddy and nerdctl/docker compose.

[1]: https://argon40.com/products/argon-one-v2-case-for-raspberry-pi-4
[2]: https://docs.armbian.com/
[3]: https://www.debian.org/releases/trixie/
[4]: https://www.dzombak.com/blog/2023/12/considerations-for-a-long-running-raspberry-pi/
[5]: https://netplan.io/
[6]: https://netplan.readthedocs.io/en/stable/howto/
