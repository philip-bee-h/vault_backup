---
title: ansible-dir-best-practice
created: 2025-05-16
type: resource
category: 
tags:
  - resource
  - tool-name
  - ansible
related_docs:
---
## **Ansible Project Directory Structure – Best Practices**

### **Recommended Structure**

```
ansible/
├── ansible.cfg
├── inventory/
│   ├── production/
│   │   ├── hosts.ini
│   │   ├── group_vars/
│   │   │   ├── all.yml
│   │   │   ├── k8s_nodes.yml
│   │   │   └── vault.yml
│   │   └── host_vars/
│   │       └── k8s-cp-01.yml
│   └── development/
│       ├── hosts.ini
│       └── group_vars/
├── roles/
│   ├── common/
│   │   ├── defaults/
│   │   ├── files/
│   │   ├── handlers/
│   │   ├── meta/
│   │   ├── tasks/
│   │   ├── templates/
│   │   └── vars/
│   ├── kubernetes/
│   └── jumphost/
├── playbooks/
│   ├── site.yml
│   ├── bootstrap.yml
│   ├── kubernetes.yml
│   └── maintenance/
│       ├── updates.yml
│       └── backup.yml
├── library/
├── filter_plugins/
├── files/
│   ├── scripts/
│   ├── personal/
│   └── utility/
├── templates/
├── vars/
│   └── main.yml
├── requirements.yml
└── README.md
```

### **Key Principles**

- **Environment Isolation**: Separate inventories for production and development.
    
- **Modular Design**: Use roles to encapsulate logic and configuration.
    
- **Variable Precedence**: Leverage `defaults`, `vars`, `group_vars`, and `host_vars`.
    
- **Playbook Clarity**: Break logic into focused playbooks, organized under `playbooks/` and tied together via `site.yml`.
    
- **Secrets Handling**: Use Vault for encrypted secrets.
    
- **Separation of Concerns**: Keep files, templates, and vars cleanly separated.
    
- **Extensibility**: Support custom modules (`library/`) and filters (`filter_plugins/`).
    
- **Dependency Management**: Track external roles in `requirements.yml`.
    
- **Documentation**: Maintain a detailed, updated `README.md`.
    

### **Benefits**

- Scalable structure for both small and large projects.
    
- Easy navigation and maintenance.
    
- Better collaboration and consistency.
    
- Clean environment separation and safer deployments.
    

