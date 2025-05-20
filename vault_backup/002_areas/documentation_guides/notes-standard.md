---
title: notes-standard
created: YYYY-MM-DD
modified: YYYY-MM-DD
type: standard
tags:
  - documentation
---
## Tips to consider


- **Favor Flat Over Deep**: Reducing folder nesting can speed up note retrieval.
- Tag meeting notes by service, e.g., `#meeting`, `#hpc`, `#idas`.



## Dir Structure - UPDATE

```shell

├── 000_inbox
│   ├── ideas
│   ├── meetings
│   └── scratch
├── 000_workbench
│
├── 001_projects
│   ├── personal-projects
│   └── work-projects
│
├── 002_areas
│   ├── documentation
│   │   ├── guides
│   │   ├── standards
│   │   └── templates
│   ├── operations
│   │   ├── hpc
│   │   │   ├── procedures
│   │   │   └── troubleshooting
│   │   ├── idas
│   │   ├── infrastructure
│   │   ├── networking
│   │   └── storage
│   ├── development
│   │   ├── certifications
│   │   ├── skills-tracking
│   │   └── technical-topics
│   │       ├── automation
│   │       ├── containerization
│   │       ├── devops
│   │       └── networking
│   └── meetings
│       ├── hpc
│       ├── idas
│       ├── storage
│       ├── manager-team
│       └── tech-talks
│
├── 003_resources
│   ├── configurations
│   │   ├── dotfiles
│   │   ├── system-configs
│   │   └── tool-configs
│   ├── tools
│   │   ├── automation
│   │   ├── editors
│   │   ├── monitoring
│   │   └── terminals
│   └── systems
│       ├── linux
│       ├── macos
│       └── windows
│
├── 004_archive
│   ├── completed-projects
│   ├── old-documentation
│   ├── past-roles
│   └── reference-history
│
├── _attachments
└── _templates
```
### Tagging Guide

Using a clear, consistent tagging strategy will help you categorize notes effectively. Here’s a structured guide based on your provided tags, organized by categories and use cases.

---

#### **Core Tag Categories**

1. **Location** – Identifies where the note or document is used or applies:
	- **`uiowa`**: For notes specific to university work.
	- **`homelab`**: For notes and configurations related to home lab setups.
	- **`personal`**: For general personal development notes or projects outside specific work contexts.

2. **System** – Specifies the system associated with the note:
	- **`hpc`**: High-Performance Computing.
	- **`storage`**: For storage system topics (e.g., Ceph, ZFS).
	- **`idas`**: IDAS - Interactive Data Analytics System
	- **`network`**: For network configurations and troubleshooting.
	- **`infrastructure`**: General infrastructure topics not specific to HPC, idas or storage.

3. **Type** – Defines the type or purpose of the document:
	- **`runbook`**: Step-by-step operational documents.
	- **`command-ref`**: Command references and guides.
	- **`troubleshooting`**: Troubleshooting guides and procedures.
	- **`meeting`**: Meeting notes and summaries.

4. **Tool** – Tags for specific tools or applications, helping to filter notes by tool:
	- **`git`**: Version control using Git.
	- **`docker`**: Container management and Docker commands.
	- **`kubernetes`**: Kubernetes cluster management.
	- **`vim`**: Editor-related configurations or commands.

5. **Status** – Tracks the lifecycle status of a note:
	- **`active`**: Currently relevant or in use.
	- **`draft`**: In development or incomplete.
	- **`pending`** or **waiting**: Awaiting action, feedback, or follow-up; not actively being worked on.
	- **`completed`**: Finished or fulfilled; may no longer need active updates.
	- **`archived`**: Old or deprecated notes that remain as references.

---

#### **Example Tags for Different Use Cases**

1. **SSH Configuration Guide**
   ```markdown
   ---
   title: SSH Configuration Guide
   tags: [ssh, security, uiowa, configuration]
   ---
   ```

2. **Storage Runbook (Ceph)**
   ```markdown
   ---
   title: Ceph Storage Setup Runbook
   tags: [runbook, storage, ceph, uiowa, active]
   ---
   ```

3. **HPC Commands Reference**
   ```markdown
   ---
   title: HPC Command Reference
   tags: [command-ref, hpc, sge, uiowa, active]
   ---
   ```

4. **Meeting Note on Networking**
   ```markdown
   ---
   title: Networking Team Meeting - October 2024
   tags: [meeting, network, uiowa, active]
   ---
   ```

Using these categories ensures each document can be quickly located and filtered by type, status, system, or tool.

---

### Quick Access Index Setup Guide

Creating an index page in Obsidian is a great way to organize and navigate your notes. Here’s a basic setup for a **Command Reference Index** and **Tag Search Index**.

---

#### **1. Command Reference Index**

```markdown
# Command Reference Directory
---
title: Command Reference Directory
type: index
---

## HPC Commands
- [[sge-commands]] - Job scheduling and management
- [[ipmi-commands]] - Node management
- [[hpc-monitoring-commands]] - System monitoring

## Storage Commands
- [[zfs-commands]] - ZFS management
- [[storcli-commands]] - Storage controller management
- [[ceph-commands]] - Ceph cluster management

## Network Commands
- [[network-commands]] - General networking commands
- [[firewall-commands]] - Firewall and security configurations

## General Tools
- [[git-commands]] - Git version control
- [[docker-commands]] - Docker commands and scripts
- [[kubernetes-commands]] - Kubernetes management
```

This index groups commands by system and general tools, making it easy to navigate to specific references.

---

#### **2. Tag-Based Search Index**

In Obsidian, you can set up a tag-based index by creating a list of clickable tags for quick filtering.

```markdown
# Search by Tag
---
title: Tag-Based Search Index
type: index
---

## Core Tags
- [#commands](obsidian://search?query=%23commands)
- [#runbook](obsidian://search?query=%23runbook)
- [#troubleshooting](obsidian://search?query=%23troubleshooting)
- [#meeting](obsidian://search?query=%23meeting)
- [#active](obsidian://search?query=%23active)
- [#archived](obsidian://search?query=%23archived)

## System-Specific Tags
- [#hpc](obsidian://search?query=%23hpc)
- [#storage](obsidian://search?query=%23storage)
- [#network](obsidian://search?query=%23network)
- [#infrastructure](obsidian://search?query=%23infrastructure)

## Tool-Specific Tags
- [#git](obsidian://search?query=%23git)
- [#docker](obsidian://search?query=%23docker)
- [#kubernetes](obsidian://search?query=%23kubernetes)
- [#vim](obsidian://search?query=%23vim)
```

Clicking each tag link will open a filtered search in Obsidian for that tag, helping you locate relevant notes quickly.

---

This setup will give you an organized, easy-to-navigate tagging and index structure in Obsidian. Let me know if you need further customization!