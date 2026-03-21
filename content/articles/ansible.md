---
title: Ansible
date: 2026-01-14
modified: 2026-03-21T18:38:27
categories: infrastructure
tags: configuration management, ansible
type: docs
draft: false
sidebar:
  open: true
---

This article collects some approaches that I have used when setting up new Ansible automation projects - it's light on explanation because it's more of an _aide-mémoire_ for myself than anything else.

## Inventory

```
inventory/
└── location1/
    ├── group_vars/
    │   └── all
    └── hosts
└── location2/
    ├── group_vars/
    │   └── all
    └── hosts
```


## Collection

Using an [Ansible Collection](https://docs.ansible.com/projects/ansible/latest/collections_guide) from a single [collection repository](https://docs.ansible.com/projects/ansible/latest/collections_guide/collections_installing.html#installing-a-collection-from-a-git-repository), I can keep all my roles in one place[^collection-repo].

```
ansible-collection/
└── rhu
    ├── host
    │   └── roles
    │       ├── 01-common
    │       ├── ansible-pull
    │       ├── backup
    │       ├── dotfiles
    │       ├── logging
    │       └── security
    ├── service
    │   └── roles
    │       ├── auth			(authentik)
    │       ├── auth-dns		(nsd)
    │       ├── backup
    │       ├── dashboard
    │       ├── dhcp-ipam
    │       ├── dns-resolver	(unbound)
    │       ├── docker
    │       ├── forge			(forgejo)
    │       ├── http
    │       ├── k8s
    │       ├── matrix
    │       ├── ntfy
    │       ├── pihole
    │       ├── reverse-proxy 	(caddy)
    │       ├── s3				(garage)
    │       ├── tunnel
    │       └── ...
--------------------------- Future --------
    ├── team1
    │   └── roles
    │       ├── appA
    │       └── appB
    └── team2
        └── roles
            ├── appC
            └── appD
```

## Roles

1. Use [Ansible Galaxy](https://docs.ansible.com/projects/ansible/latest/galaxy/dev_guide.html)[^ag-cli] to create a conventional set of files.

  	```shell
  	ansible-galaxy role init [--role-skeleton=/path/to/skeleton] role_name
  	```
2.

### Testing within a Role/Playbook

There are several useful patterns that can help with testing[^test-strat].

* `wait_for`:
    ```yaml
    tasks:

      - ansible.builtin.wait_for:
          host: "{{ inventory_hostname }}"
          port: 22
        delegate_to: localhost
    ```
* `uri`:
    ```yaml
    tasks:

      - action: uri url=https://www.example.com return_content=yes
        register: webpage

      - fail:
          msg: 'service is not happy'
        when: "'AWESOME' not in webpage.content"
    ```
* `assert`:
    ```yaml
    tasks:

      - ansible.builtin.shell: /usr/bin/some-command --parameter value
        register: cmd_result

      - ansible.builtin.assert:
          that:
            - "'not ready' not in cmd_result.stderr"
            - "'gizmo enabled' in cmd_result.stdout"
    ```


[^test-strat]: Lifted from https://docs.ansible.com/projects/ansible/latest/reference_appendices/test_strategies.html#modules-that-are-useful-for-testing


### Testing Roles

TODO: Return to testing with [molecule](https://docs.ansible.com/projects/molecule/)

[^ag-cli]: the CLI that makes it easier to work with Roles, not the [Ansible Galaxy website](https://galaxy.ansible.com/) which is a service for sharing Ansible automation content.

## See Also

* My [tips:ansible](../tips-ansible/)

[^collection-repo]: (TODO: publish Collection to <domain>/ansible-collection)
