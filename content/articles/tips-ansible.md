---
title: Ansible Tips
date: 2025-08-13
modified: 2025-09-06T13:13:45
categories: infrastructure
tags: configuration management, ansible, tips
type: docs
draft: false
sidebar:
  open: true
---

This article is to collect some reminders of certain Ansible syntax/techniques that I have forgotten more than once.
This article is neither an introduction to Ansible, nor an apologia/panegyric/paean for Ansible, nor a replacement for the project's (unusually good) [documentation][1].

1. Run a playbook vs. Run an ad-hoc command:
    ```console
    ansible-playbook main.yml -i inventory/my-inv-file -K -l <some_hostname_or_inventory_group>
    ansible all -i inventory/my-inv-file -K <some_hostname_or_inventory_group> -m ansible.builtin.shell --args "uname -a"
    ````

1. There are lots of different ways to have Ansible Playbooks run commands, include various different syntaxes for the same Ansible module in some cases. Mostly the other forms for the `command` module are syntactic sugar, so I try to limit myself to this.
    ```yaml
    - name: an example of the command module
      ansible.builtin.command:
        argv:
          - /path/to/binary
          - --option
          - --option-with-value=foo
      when:
        - some_condition
        - not some_other_condition
      register: example_task
    ```

1. Another approach that I commonly use is the `shell` module. This is for when I want pipes, (file) redirects, and other shell niceties.
    ```yaml
    - name: an example of the shell module
      ansible.builtin.shell: echo foo | tee this_gets_overwritten.txt >> this_gets_appended_to.txt
      args:
        executable: bash
    ```

1. For a task, check for group membership; `'group_name'` can be a string literal or a variable.
    ```yaml
    - name: conditional task
      ansible.builtin.command:
        cmd: echo "I _am_ in group: {{ item }}"

      with_items:
        - server
      when: inventory_hostname in groups[item]
      register: conditional_task
    ```

1. For the above task, show its output:
    ```yaml
    - name: Show the conditional task's output
      ansible.builtin.debug:
        msg: "{{ conditional_task.results[0].stdout_lines }}"
      when:
        - conditional_task.changed
    ```


## See Also

* Special Variables: https://docs.ansible.com/ansible/latest/reference_appendices/special_variables.html
* `ansible.builtin.command`: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#notes
* `ansible.builtin.shell`: https://docs.ansible.com/ansible/latest/collections/ansible/builtin/shell_module.html#notes
* [Ansible documentation][1]

[1]: https://docs.ansible.com
