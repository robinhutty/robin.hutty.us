---
title: Provisioning Virtual Machines and initial Configuration Management
date: 2025-08-10
categories: automation
tags: cicd
type: docs
draft: true
sidebar:
  open: true
---

NOTE: This article is deliberately scoped to provisioning on hardware, i.e. provisioning into Cloud platforms and such IaaS[^IaaS] is _out of scope_. I have other articles about Terraform and other technologies that I use for interacting with Cloud Providers (public and private), which latter has been much more common for me for more than a decade.

For many years, I have written code that manages linux/unix machines, supporting many versions of several linux distributions and quite a few unix-like OSen that are not linux. Back before we used the terms "Infrastructure as Code" or "DevOps", back when even the term "Configuration Management" was not ubiquitous. I have done this in both my professional world - for several employers and clients - and for my private computing purposes. But it's been a long time since I built/ran automated installation services for _not-IaaS in_ any meaningful way and presumably things have changed. So my intention for this article is to collect what I'm up to this time around.

The main use case is: I want to have a _trivial_ process for provisioning Virtual Machines in my homelab; most of which will be ephemeral (or at least fairly short-lived) machines for working on/with other automation efforts, such as "update the configuration management codebase to support a new major version of a distro, or a new distro, or ..."

Primary goals:

1. Code that provisions a new machine on (hypervisor or VM manager)
1. When a new machine is created, it goes through an unattended installation of an OS and installs one or more public SSH keys so that the machine can participate in Configuration Management.

Secondary goals:

1. Post-install that updates the Configuration Management Inventory to record at least the existence of a new machine
1. Post-install that calls the Configuration Management system so that the new machine gets configured to suit its intended purpose
1. Support for multiple linux distributions/versions/architecture (first debian 13/trixie on amd64, and branching out from there)
1. Integration with IPAM
1. Continuous Integration testing for the entire process to provide confidence that the process will succeed or alerts me that the process has bit-rotted enough that it needs some attention
1. Local mirrors/archives for debian (amd64/arm64), homebrew, etc.

## A minimal repository for ansible-pull

As the last step of installing OSen (and/or upon first boot), I'd like to have the machine use Ansible to configure itself (instead of an additional manual step where I update my Ansible Inventory and manually initiate a standard (push-based) Ansible run).

The goal is to support a command like:

```shell
ansible-pull -U https://git.example.com/my-ansible-repo.git -i "$(hostname --short),"
```

See TODO

## Provisioning

In the distant past, I've used many different technologies for provisioning virtual machines): various libvirt-based tools (including custom code I've written), vagrant, UTM, plus the native tools from each hypervisor/virtualization framework (QEMU/KVM, proxmox, Hyper-V, Xen, ESXi, openvz, bhyve, Apple VZ).

For this generation, I'm using:

* Debian as the host OS
* [QEMU][1]/[linux-kvm][2]
* [lima][3] to hide some of the details of working with qemu/kvm



[^iso]: I could perhaps use a simpler process that one-off modifies the installation image to provide some or all of the configuration data.
[^IaaS]: I've done professional work with: AWS, OCI, OpenStack, DO, GCP, Azure, and probably others I'm forgetting

--------

## Using lima on a debian(13) host

```console
$ brew install lima lima-additional-guestagents
$ sudo apt install libguestfs-tools
```

### lima-on-linux-template-debian13.yaml

```yaml
minimumLimaVersion: 1.1.0

base: template://_default/mounts
vmType: qemu
networks:
  - lima: bridged

## Want to specify DNS servers for the new VM?
#dns:
#  - 192.168.1.3
#  - 1.1.1.1
#useHostResolver: false

# CPUs
# Builtin default: min(4, host CPU cores)
cpus: null

# Memory size
# Builtin default: min("4GiB", half of host memory)
memory: null

# Disk size
# Builtin default: "100GiB"
disk: null

images:
- location: http://localhost:8000/my-debian-13-genericcloud-amd64-daily.qcow2
  arch: x86_64

provision:
  - mode: user
    script: |
      #!/bin/bash
      # This script should be idempotent and otherwise robust
      set -eux -o pipefail
      # curl -fsSL https://robin.hutty.us/ssh_public_key.pub -o ~robin/.ssh/robinhutty.pub --no-clobber
      # chmod 0644 ~robin/.ssh/robinhutty.pub

  mountTypesUnsupported: [9p]
```

### `~/.lima/_config/networks.yaml`

```yaml
paths:
# socketVMNet requires Lima >= 0.12 .
  socketVMNet: "/opt/socket_vmnet/bin/socket_vmnet"
  varRun: /private/var/run/lima
  sudoers: /private/etc/sudoers.d/lima

group: everyone

networks:
  user-v2:
    mode: user-v2
    gateway: 192.168.104.1
    netmask: 255.255.255.0
  shared:
    mode: shared
    gateway: 192.168.105.1
    dhcpEnd: 192.168.105.254
    netmask: 255.255.255.0
  bridged:
    mode: bridged
    interface: eno1
    # bridged mode doesn't have a gateway; dhcp is managed by outside network
  host:
    mode: host
    gateway: 192.168.106.1
    dhcpEnd: 192.168.106.254
    netmask: 255.255.255.0
```

Yep, this is only for a nice, safe, homelab network - Ansible will change the root password, prevent root login, etc.

Fetch an existing image as a starting point:

```console
$ curl -OfsSLv http://cloud.debian.org/images/cloud/trixie/daily/latest/debian-13-genericcloud-amd64-daily.qcow2
$ cp debian-13-genericcloud-amd64-daily.qcow2 my-debian-13-genericcloud-amd64-daily.qcow2
```

Customize it[^virt-customize]:

```console
$ virt-customize --add my-debian-13-genericcloud-amd64-daily.qcow2 \
  --root-password password:changeme \
  --ssh-inject root:./id_rsa.pub \
  --install curl,sudo,ansible \
  --update \
  --hostname "vm-debian13-amd64$(dnsdomainname)" \
  # --append-line "/etc/ssh/sshd_config.d/rootlogin.conf":"PermitRootLogin yes" \
  # --append-line "/etc/ssh/sshd_config.d/rootlogin.conf":"PasswordAuthentication yes" \
  # --run-command \
  #   'useradd -G sudo -m -p "changeme" robin; ' \
  # --firstboot-command 'ansible-pull -U https://git.example.com/my-ansible-repo.git -i "$(hostname --short),"' \
  --firstboot-command 'ssh-keygen -A && systemctl restart sshd'

$ busybox httpd -f -p 8000

$ limactl create --name="vm-debian13-amd64" \
  http://localhost:8000/lima-on-linux-template-debian13.yaml

<snip>
INFO[0034] Run `limactl start vm-debian13-amd64` to start the instance.

$ limactl start vm-debian13-amd64

<snip>
INFO[0057] READY. Run `limactl shell vm-debian13-amd64` to open the shell.

$ limactl shell vm-debian13-amd64

```

[^virt-customize]: See the [man page][4] for `virt-customize` for details about available options.

## Finalizing a Debian preseed

```debconf
d-i preseed/late_command string echo "Finalize: Run CM"; ansible-pull -U https://git.example.com/my-ansible-repo.git -i "$(hostname --short),"
```

## See Also

* https://www.virt-tools.org
* https://wiki.qemu.org/Documentation/Networking

[1]: https://www.qemu.org
[2]: https://linux-kvm.org/page/HOWTO
[3]: https://lima-vm.io
[4]: https://libguestfs.org/virt-customize.1.html
