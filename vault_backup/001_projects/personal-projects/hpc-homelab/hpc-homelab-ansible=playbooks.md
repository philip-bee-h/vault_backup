---
title: "hpc-homelab-ansible=playbooks"
created: "2025-05-18"
type: project
status: 
tags:
  - project
  - area-tag
  - uiowa
related_docs:
---
Here's a **basic Ansible setup** to automate deployment of your small home HPC cluster with:

- Munge
    
- NFS
    
- Slurm
    
- Basic configuration management
    

You can expand this later to include Open OnDemand and Lmod.

---

## **Directory Structure**

```
hpc-cluster/
├── ansible.cfg
├── inventory/
│   └── hosts.ini
├── playbooks/
│   ├── setup-nfs.yml
│   ├── setup-munge.yml
│   ├── setup-slurm.yml
│   └── setup-users.yml
├── roles/
│   ├── nfs/
│   ├── munge/
│   └── slurm/
```

---

## **1. `ansible.cfg`**

```ini
[defaults]
inventory = inventory/hosts.ini
remote_user = your_ssh_user
host_key_checking = False
```

---

## **2. `inventory/hosts.ini`**

```ini
[controller]
head ansible_host=192.168.1.10

[compute]
node01 ansible_host=192.168.1.11
node02 ansible_host=192.168.1.12

[all:vars]
ansible_python_interpreter=/usr/bin/python3
```

---

## **3. `playbooks/setup-nfs.yml`**

```yaml
---
- name: Set up NFS server (controller)
  hosts: controller
  become: true
  tasks:
    - name: Install NFS server packages
      package:
        name: nfs-utils
        state: present

    - name: Configure /etc/exports
      copy:
        dest: /etc/exports
        content: "/home *(rw,sync,no_subtree_check,no_root_squash)\n"

    - name: Enable and start NFS server
      service:
        name: nfs-server
        state: started
        enabled: true

- name: Set up NFS client (compute nodes)
  hosts: compute
  become: true
  tasks:
    - name: Install NFS client packages
      package:
        name: nfs-utils
        state: present

    - name: Create /home mount point
      file:
        path: /home
        state: directory

    - name: Mount /home from controller
      mount:
        path: /home
        src: "{{ hostvars['head']['ansible_host'] }}:/home"
        fstype: nfs
        opts: defaults
        state: mounted
```

---

## **4. `playbooks/setup-munge.yml`**

```yaml
---
- name: Install and configure Munge
  hosts: all
  become: true
  tasks:
    - name: Install Munge
      package:
        name: munge
        state: present

    - name: Copy munge.key from controller
      fetch:
        src: /etc/munge/munge.key
        dest: files/munge.key
      when: inventory_hostname == 'head'

    - name: Distribute munge.key
      copy:
        src: files/munge.key
        dest: /etc/munge/munge.key
        owner: munge
        group: munge
        mode: '0400'

    - name: Enable and start Munge
      service:
        name: munge
        state: started
        enabled: true
```

> **Note:** The `fetch` step above assumes you manually installed `munge` on the controller and generated the key with `create-munge-key`.

---

## **5. `playbooks/setup-slurm.yml`**

```yaml
---
- name: Install and configure Slurm
  hosts: all
  become: true
  vars:
    slurm_user: slurm
    slurm_group: slurm
  tasks:
    - name: Install Slurm packages
      package:
        name:
          - slurm
          - slurm-slurmd
          - slurm-slurmctld
        state: present

    - name: Create slurm group and user
      group:
        name: "{{ slurm_group }}"
        state: present

    - name: Ensure slurm user exists
      user:
        name: "{{ slurm_user }}"
        group: "{{ slurm_group }}"
        create_home: no
        shell: /sbin/nologin

    - name: Deploy slurm.conf
      copy:
        src: files/slurm.conf
        dest: /etc/slurm/slurm.conf
        owner: root
        group: root
        mode: '0644'

- name: Start Slurm services
  hosts: controller
  become: true
  tasks:
    - name: Start slurmctld
      service:
        name: slurmctld
        state: started
        enabled: true

- name: Start Slurm compute daemon
  hosts: compute
  become: true
  tasks:
    - name: Start slurmd
      service:
        name: slurmd
        state: started
        enabled: true
```

---

## **6. `playbooks/setup-users.yml`**

```yaml
---
- name: Create common HPC users
  hosts: all
  become: true
  vars:
    users:
      - { name: "hpcuser", uid: 1001 }
  tasks:
    - name: Create users
      user:
        name: "{{ item.name }}"
        uid: "{{ item.uid }}"
        shell: /bin/bash
        state: present
      loop: "{{ users }}"
```

---

## **Next Steps**

- Test this setup on 2–3 VMs or physical nodes
    
- Add Open OnDemand or Lmod roles later
    
- Optionally: use templates for `slurm.conf` if you want to parameterize it
    

---

Would you like me to provide:

- A sample `slurm.conf` file?
    
- Role-based Ansible structure instead of flat playbooks?
    
- Open OnDemand integration playbook?
    

Let me know and I’ll build that out for you.