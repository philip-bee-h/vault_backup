---
title: running-ansible-commands
created: 2025-05-16
type: command-reference
system: 
tags:
  - commands
  - ansible
related_runbooks:
---


# Ansible Quick Reference Guide

## Basic Commands

```bash
# Run a playbook
ansible-playbook playbook.yml

# Run on specific hosts/groups
ansible-playbook playbook.yml --limit webservers
ansible-playbook playbook.yml --limit "server1,server2"
ansible-playbook playbook.yml --limit "*.example.com"

# Run with extra variables
ansible-playbook playbook.yml -e "ansible_user_password=SecurePassword123"
ansible-playbook playbook.yml --extra-vars "env=prod version=1.5"

# Check mode (dry run)
ansible-playbook playbook.yml --check

# Check syntax only
ansible-playbook playbook.yml --syntax-check

# Run specific tasks using tags
ansible-playbook playbook.yml --tags "configuration,packages"
ansible-playbook playbook.yml --skip-tags "restart,notification"
```

## Inventory Management

```bash
# List all hosts in inventory
ansible all --list-hosts

# List specific group hosts
ansible webservers --list-hosts

# Show host variables
ansible host1 -m debug -a "var=hostvars[inventory_hostname]"

# Ping all hosts to check connectivity
ansible all -m ping

# Run ad-hoc command on hosts
ansible webservers -m shell -a "uptime"
ansible all -m apt -a "name=vim state=present" --become
```

## File Structure Reference

```
project/
├── inventory
│   ├── hosts                 # Main inventory file
│   └── group_vars/           # Group variables
│       ├── all.yml           # Variables for all hosts
│       └── webservers.yml    # Variables for webserver group
├── playbooks/
│   ├── site.yml              # Main playbook
│   ├── webservers.yml        # Webservers specific tasks
│   └── dbservers.yml         # Database servers specific tasks
└── files/                    # Files to be copied to hosts
    ├── personal/             # Personal configuration files
    │   └── .bashrc           # Full featured bashrc
    └── utility/              # Utility/minimal files
        └── .bashrc           # Minimal bashrc
```

## Common Playbook Snippets

### Bootstrap Ansible User

```yaml
- name: Create ansible user
  user:
    name: ansible
    shell: /bin/bash
    state: present
    create_home: yes

- name: Add SSH key for ansible user
  authorized_key:
    user: ansible
    state: present
    key: "{{ lookup('file', '~/.ssh/id_rsa_ansible.pub') }}"

- name: Configure passwordless sudo
  copy:
    dest: /etc/sudoers.d/ansible
    content: "ansible ALL=(ALL) NOPASSWD: ALL\n"
    mode: "0440"
```

### Update Package Cache and Upgrade

```yaml
- name: Update apt cache
  apt:
    update_cache: yes
    cache_valid_time: 3600

- name: Upgrade packages
  apt:
    upgrade: dist
    autoremove: yes
```

### Deploy Dotfiles

```yaml
- name: Find dotfiles in directory
  find:
    paths: "{{ playbook_dir }}/files/utility"
    patterns: ".*"
    hidden: yes
  register: dotfiles_found
  delegate_to: localhost
  become: no

- name: Copy dotfiles
  copy:
    src: "{{ item[1].path }}"
    dest: "/home/{{ item[0] }}/{{ item[1].path | basename }}"
    owner: "{{ item[0] }}"
    group: "{{ item[0] }}"
    mode: '0644'
    backup: yes
  loop: "{{ users | product(dotfiles_found.files) | list }}"
```

## Troubleshooting

```bash
# Increase verbosity for debugging
ansible-playbook playbook.yml -v       # Verbose
ansible-playbook playbook.yml -vv      # More verbose
ansible-playbook playbook.yml -vvv     # Debug level
ansible-playbook playbook.yml -vvvv    # Connection level debugging

# Show all facts gathered for a host
ansible hostname -m setup

# Display only specific facts
ansible hostname -m setup -a "filter=ansible_distribution*"

# Start at specific task
ansible-playbook playbook.yml --start-at-task="Install packages"
```