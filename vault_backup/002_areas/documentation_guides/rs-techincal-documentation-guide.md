---
title: rs-techincal-documentation-guide
created: 2025-04-07
type: resource
category: 
tags:
  - resource
  - documentation
related_docs:
---
# **Technical Documentation Guide for Research Computing & Infrastructure Teams**

## **Purpose**

This guide outlines best practices for writing, organizing, and maintaining documentation across domains such as:

- Research computing (HPC clusters, job schedulers)
    
- Linux systems administration
    
- Storage systems (ZFS, Ceph, etc.)
    
- DevOps practices (CI/CD, automation, containers)
    
- Infrastructure and service support
    

---

## **1. Why Documentation Matters in Research IT**

### Documentation Prevents:

- Knowledge silos
    
- Repeated troubleshooting of the same problems
    
- On-call guesswork
    
- Misconfigurations and failed maintenance windows
    

### Documentation Enables:

- Faster onboarding of new team members
    
- Efficient troubleshooting during incidents
    
- Standardized, repeatable service management
    
- Cross-team collaboration and autonomy
    

---

## **2. Documentation Principles**

Follow these core principles for every document:

|Principle|Description|
|---|---|
|**Clear**|Use plain language, avoid jargon unless necessary|
|**Actionable**|Include step-by-step instructions with examples|
|**Accurate**|Reflect current environments and processes|
|**Organized**|Use headers, bullets, and clear formatting|
|**Accessible**|Store docs in a version-controlled, searchable location|
|**Maintained**|Assign ownership and set review intervals|

---

## **3. Types of Documentation**

### a. **Runbooks (Operational Procedures)**

**Examples:**

- Restarting a Slurm daemon
    
- Adding a user to LDAP/NFS
    
- Upgrading a Kubernetes cluster
    
- Performing a Ceph maintenance operation
    

**Template:**

```markdown
# Task Title: Restart Slurm Controller

## Prerequisites
- SSH access to head node
- Backup of current config: `/etc/slurm/slurm.conf`

## Steps
1. `sudo systemctl stop slurmctld`
2. Verify: `systemctl status slurmctld`
3. `sudo systemctl start slurmctld`
4. Check log output: `journalctl -u slurmctld`

## Validation
- Submit a test job with `sbatch`
- Confirm job runs successfully
```

---

### b. **Architecture & Infrastructure Documentation**

**Purpose:** Describe how systems are built and interconnect.

**Include:**

- System diagrams (e.g., HPC cluster layout, Ceph topology)
    
- Deployment tools (e.g., Puppet/Ansible structure)
    
- Networking models and interfaces
    
- Storage policies, quota rules, redundancy setup
    

---

### c. **Incident Reports / Postmortems**

**Use case:** Capture failures and lessons learned.

**Format:**

```markdown
# Incident: Lustre Filesystem Outage – 2025-03-01

**Summary**
Metadata target server crashed, causing I/O failures.

**Impact**
All compute jobs depending on /lustre were affected (approx. 300 users).

**Resolution**
- Rebooted MDS
- Ran fsck
- Restarted OSSes

**Prevention**
- Scheduled monthly health checks
- Enabled HA failover node for MDS
```

---

### d. **Service Catalog Entries**

**Purpose:** Provide an overview of managed services.

**Include:**

- Description of the service
    
- Who uses it (research groups, internal tools)
    
- How to request access
    
- Contact or escalation info
    
- Related documentation
    

---

### e. **Deployment & CI/CD Documentation**

**Purpose:** Describe how code/config changes move through environments.

**Include:**

- Git repo structure and branching policies
    
- Pipeline stages (build → test → deploy)
    
- Automation tools (Ansible, Jenkins, GitHub Actions)
    
- Manual steps (if any)
    

---

## **4. Writing Style Guide**

### Keep Language Simple and Direct

- Write for the next admin who doesn’t have full context
    
- Prefer clarity over cleverness
    

### Use Examples Wherever Possible

- Show commands and expected output
    

```bash
ansible-playbook site.yml --limit compute_nodes
```

### Be Consistent

- Standardize document naming (`runbook-<task>.md`)
    
- Use common sections: Prerequisites, Steps, Validation, Troubleshooting
    

### Use Visuals for Complex Concepts

- Diagrams for networking/storage/HPC layouts
    
- Annotated screenshots or flowcharts when helpful
    

---

## **5. Organizing and Storing Documentation**

### Recommended Structure

```
/infrastructure-docs
├── runbooks/
│   ├── restart-slurm.md
│   ├── ceph-maintenance.md
├── architecture/
│   ├── hpc-topology.png
│   └── ceph-cluster-layout.md
├── incidents/
│   ├── 2025-03-01-lustre-outage.md
├── services/
│   ├── research-storage.md
│   └── login-node-policy.md
├── ci-cd/
│   └── deploy-infra-guide.md
```

### Where to Store Docs

|Tool|Best For|
|---|---|
|Git (Markdown)|Versioned, developer-facing content|
|Confluence/Wiki|Internal IT policy, service overviews|
|Backstage, MkDocs|Searchable web UIs for technical docs|
|README in Repos|CI/CD, automation scripts, configs|

---

## **6. Maintenance and Ownership**

- Assign documentation owners per service/team
    
- Use review schedules (quarterly or post-change)
    
- Link documentation updates to change tickets or code merges
    
- Mark outdated docs clearly (use banners or tags)
    

---

## **7. Documentation Checklist**

Use this before publishing any doc:

-  Clear title and purpose
    
-  Written in plain, direct language
    
-  Has commands or config snippets if needed
    
-  Steps are verified and complete
    
-  Contains validation or troubleshooting tips
    
-  Document is versioned and stored centrally
    
-  Owner is assigned for review
    

---

## **Final Thoughts**

Documentation is not an afterthought—it's an operational asset. It supports continuity, empowers your team, and reduces reliance on tribal knowledge. Invest in it like you would any other critical infrastructure.
