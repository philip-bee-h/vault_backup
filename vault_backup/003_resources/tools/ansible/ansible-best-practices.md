# Ansible Best Practices Guide

## Directory Structure

The ideal Ansible project structure follows these conventions:

```
ansible/
├── ansible.cfg             # Configuration file
├── inventory.ini           # Inventory file
├── files/                  # Static files
│   ├── personal/           # Personal dotfiles
│   │   └── .bashrc
│   └── utility/            # Minimal dotfiles for servers
│       └── .bashrc
├── group_vars/             # Group variables
│   ├── all/                # Variables for all hosts
│   │   └── main.yml
│   ├── k8_dev_nodes/       # Variables for dev environment
│   │   └── main.yml
│   └── k8_prod_nodes/      # Variables for prod environment
│       └── main.yml
├── host_vars/              # Host-specific variables
│   └── k8s-cp-01.yml       # Example: control plane host 
├── playbooks/              # Task playbooks
│   ├── ansible_user_bootstrap.yml
│   ├── dotfiles_personal.yml
│   ├── dotfiles_utility.yml
│   ├── ubu_svr_bootstrap.yml
│   └── site.yml            # Main playbook entry point
├── roles/                  # Reusable roles
│   ├── common/             # Common configurations
│   │   ├── defaults/
│   │   │   └── main.yml
│   │   ├── files/
│   │   ├── handlers/
│   │   │   └── main.yml
│   │   ├── tasks/
│   │   │   └── main.yml
│   │   └── templates/
│   └── kubernetes/         # Kubernetes specific role
└── README.md               # Project documentation
```

## Configuration Best Practices

### ansible.cfg

A basic ansible.cfg file should include:

```ini
[defaults]
# Inventory file location
inventory = inventory.ini
# Disable host key checking for lab environments
host_key_checking = False
# Enable color output
force_color = True
# Parallelism - increase for larger environments
forks = 10
# Shows timing information
callbacks_enabled = timer, profile_tasks
# Python interpreter discovery
interpreter_python = auto
# Default user to use for playbooks
remote_user = ansible

[privilege_escalation]
# Default settings for privilege escalation
become = True
become_method = sudo
```

### Using Variables

Variables should be organized based on their scope:

**group_vars/all/main.yml** (applies to all hosts):
```yaml
# Common settings for all servers
ntp_servers:
  - 0.pool.ntp.org
  - 1.pool.ntp.org

common_packages:
  - vim
  - htop
  - tmux
  - git
```

**group_vars/k8_prod_nodes/main.yml** (only for production):
```yaml
# Production-specific settings
env: production
k8s_version: 1.27
pod_network_cidr: 10.244.0.0/16
deploy_monitoring: true
```

**host_vars/k8s-cp-01.yml** (specific to one host):
```yaml
# Control plane specific settings
is_control_plane: true
control_plane_endpoint: 192.168.20.10
```

## Roles for Code Reuse

Roles package tasks, handlers, variables, files, templates, and more into reusable units.

### Creating a Basic Role

```bash
# Create a basic role structure
mkdir -p roles/common/{tasks,handlers,defaults,files,templates,vars}
touch roles/common/{tasks,handlers,defaults,vars}/main.yml
```

**roles/common/tasks/main.yml**:
```yaml
---
- name: Update apt cache
  apt:
    update_cache: yes
    cache_valid_time: 3600
  tags: packages

- name: Install common packages
  apt:
    name: "{{ common_packages }}"
    state: present
  tags: packages
  notify: Restart services if needed

- name: Configure SSH hardening
  template:
    src: sshd_config.j2
    dest: /etc/ssh/sshd_config
    mode: 0644
    validate: '/usr/sbin/sshd -t -f %s'
  tags: security
  notify: Restart SSH
```

**roles/common/handlers/main.yml**:
```yaml
---
- name: Restart SSH
  service:
    name: sshd
    state: restarted

- name: Restart services if needed
  command: /bin/true
  changed_when: false
  notify:
    - Restart SSH
```

**roles/common/defaults/main.yml**:
```yaml
---
# Default packages (can be overridden by group_vars)
common_packages:
  - vim
  - curl
  - htop

# SSH settings
ssh_permit_root_login: "no"
ssh_password_authentication: "no"
```

### Using Roles in Playbooks

```yaml
---
- name: Apply common configuration
  hosts: all_nodes
  roles:
    - common
```

## Playbook Organization

### Bootstrap Playbook

For creating an ansible user on new servers:

```yaml
---
- name: Bootstrap a new server for Ansible
  hosts: all_nodes
  become: yes
  vars:
    ansible_user_name: ansible
    ssh_key_path: "~/.ssh/id_rsa_ansible.pub"
  tasks:
    - name: Create ansible user
      user:
        name: "{{ ansible_user_name }}"
        shell: /bin/bash
        state: present
        create_home: yes
        password: "{{ ansible_user_password | password_hash('sha512') if ansible_user_password is defined else omit }}"
        
    - name: Add SSH key
      authorized_key:
        user: "{{ ansible_user_name }}"
        state: present
        key: "{{ lookup('file', ssh_key_path) }}"
        
    - name: Allow ansible user passwordless sudo
      copy:
        dest: /etc/sudoers.d/{{ ansible_user_name }}
        content: "{{ ansible_user_name }} ALL=(ALL) NOPASSWD: ALL\n"
        mode: "0440"
```

### System Update Playbook

For updating packages on Ubuntu servers:

```yaml
---
- name: Update and configure Ubuntu servers
  hosts: all_nodes
  become: yes
  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes
        cache_valid_time: 3600

    - name: Upgrade all packages
      apt:
        upgrade: full
        autoclean: true
        autoremove: true

    - name: Install common packages
      apt:
        name: "{{ common_packages }}"
        state: present

    - name: Check if reboot is required
      stat:
        path: /var/run/reboot-required
      register: reboot_required_file
      
    - name: Restart if required
      reboot:
        msg: "Reboot required after system updates"
        connect_timeout: 5
        reboot_timeout: 300
        pre_reboot_delay: 0
        post_reboot_delay: 30
        test_command: uptime
      when: ansible_pkg_mgr == 'apt' and reboot_required_file.stat.exists
```

### Dotfiles Management

For managing dotfiles across servers:

```yaml
---
- name: Deploy utility dotfiles to worker nodes
  hosts: k8_dev_nodes:k8_prod_nodes
  become: true
  vars:
    users:
      - pbh
      - ansible
  tasks:
    - name: Find all dotfiles in utility directory
      find:
        paths: "{{ playbook_dir }}/../files/utility"
        patterns: ".*"
        hidden: true
      register: dotfiles_found
      delegate_to: localhost
      become: false
      
    - name: Copy utility dotfiles for each user
      copy:
        src: "{{ item[1].path }}"
        dest: "/home/{{ item[0] }}/{{ item[1].path | basename }}"
        owner: "{{ item[0] }}"
        group: "{{ item[0] }}"
        mode: "0644"
        backup: true
      loop: "{{ users | product(dotfiles_found.files) | list }}"
      when: dotfiles_found.files | length > 0
```

### Main Entry Point (site.yml)

Create a main playbook that imports all others:

```yaml
---
# Main playbook - entry point for applying all configurations
- name: Apply common configuration
  hosts: all_nodes
  roles:
    - common

- name: Deploy utility dotfiles
  import_playbook: dotfiles_utility.yml

- name: Deploy personal dotfiles
  import_playbook: dotfiles_personal.yml
```

## Advanced Playbooks

### Kubernetes Control Plane Setup

```yaml
---
- name: Configure Kubernetes Control Plane
  hosts: k8s_control_planes
  become: true
  tasks:
    - name: Initialize the Kubernetes cluster
      command: >
        kubeadm init 
        --pod-network-cidr={{ pod_network_cidr }} 
        --control-plane-endpoint={{ control_plane_endpoint }}
      args:
        creates: /etc/kubernetes/admin.conf
      register: kubeadm_init
      
    - name: Create .kube directory for user
      file:
        path: "/home/{{ ansible_user }}/.kube"
        state: directory
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
        mode: '0755'
        
    - name: Copy kube admin config
      copy:
        src: /etc/kubernetes/admin.conf
        dest: "/home/{{ ansible_user }}/.kube/config"
        remote_src: yes
        owner: "{{ ansible_user }}"
        group: "{{ ansible_user }}"
        mode: '0600'
```

### Kubernetes Worker Setup

```yaml
---
- name: Join Kubernetes Workers to Cluster
  hosts: k8s_workers
  become: true
  tasks:
    - name: Get join command from control plane
      command: kubeadm token create --print-join-command
      delegate_to: "{{ groups['k8s_control_planes'][0] }}"
      register: join_command
      changed_when: false
      
    - name: Join worker to the cluster
      command: "{{ join_command.stdout }}"
      args:
        creates: /etc/kubernetes/kubelet.conf
```

## Common Ansible Commands

```bash
# Run a playbook
ansible-playbook playbooks/site.yml

# Run on specific hosts/groups
ansible-playbook playbooks/ansible_user_bootstrap.yml --limit k8_prod_nodes

# Run with extra variables
ansible-playbook playbooks/bootstrap.yml -e "ansible_user_password=SecurePass123"

# Check mode (dry run)
ansible-playbook playbooks/ubu_svr_bootstrap.yml --check

# Run with passwords
ansible-playbook playbooks/bootstrap.yml --ask-pass --ask-become-pass

# List all hosts in inventory
ansible all --list-hosts

# Ping all hosts to check connectivity
ansible all -m ping

# Run ad-hoc command on hosts
ansible webservers -m shell -a "uptime"
```

## Implementation Strategy

1. **Start Small**: Begin with core functionality like bootstrap and update playbooks
2. **Add Structure**: Implement group_vars and host_vars for variable organization
3. **Create Roles**: Refactor common tasks into reusable roles
4. **Expand Coverage**: Add specialized playbooks for specific services
5. **Centralize Entry Points**: Create a site.yml that ties everything together

## Practical Tips

1. **Always Use Check Mode First**: `--check` flag simulates changes
2. **Tag Your Tasks**: Use tags for selective execution
3. **Use Vault for Secrets**: `ansible-vault create group_vars/all/vault.yml`
4. **Test Connectivity**: Use `ansible all -m ping` to verify inventory
5. **Increase Verbosity**: Use `-v`, `-vv`, or `-vvv` for debugging
6. **Validate Templates**: Use `validate` parameter when possible
7. **Handle Errors Gracefully**: Use `failed_when` and `changed_when`
8. **Document Your Code**: Maintain a clear README and use comments

This guide covers the fundamental best practices for organizing and maintaining an Ansible project, with a focus on infrastructure management for Kubernetes environments.
