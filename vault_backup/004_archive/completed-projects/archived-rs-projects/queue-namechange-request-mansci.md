---
title: queue-namechange-request-mansci
status: follow up
tags: 
date: 2024-07-15
ticketMade: false
---

### the request

> **From:** Heil, Brian W <[brian-heil@uiowa.edu](mailto:brian-heil@uiowa.edu "mailto:brian-heil@uiowa.edu")>  
> **Sent:** Wednesday, June 26, 2024 9:11 AM  
> **To:** Research Computing Support <[research-computing@uiowa.edu](mailto:research-computing@uiowa.edu "mailto:research-computing@uiowa.edu")>  
> **Subject:** Weird queue question
> 
> We have 2 HPC queues called MANSCI and MANSCI-GPU and we are wondering if it is possible to change the queue names? The department is now known as Business Analytics & Information Sciences, so the request would be to change the queues to BAIS and BAIS-GPU.
> 
> Thanks!

### In Access Management 
MANSCI > BAIS
MANSCI Exceptions > BAIS Exceptions
### In AD
its-rs-MANSCI > its-rs-BAIS
its-rs-MANSCI-allowlist > its-rs-BAIS-allowlist
its-rs-MANSCI-denylist > its-rs-BAIS-denylis

### IAM response

#### "its-rs-MANSCI" Group
- **Management Mechanism**: Managed by one of our business rules.
- **Use of ObjectGuids**: The use of ObjectGuids means that renaming the group in Active Directory (AD) will not affect our ability to auto-manage the population.

#### "its-rs-MANSCI-{allow|deny}list" Groups
- **Management Location**: Managed in Access Management.
- **Use of ObjectGuids**: Similar to the "its-rs-MANSCI" group, renaming in AD will not affect our management abilities.
- **Additional Action Required**: AM will not auto-update when the AD group is renamed, so you will need to update AM separately.

## Original yaml
### MANSCI
```yaml
  # name: Qihang Lin
  # email: qihang-lin@uiowa.edu
  sge::config::queue {'MANSCI':
    hostlist   => ['@MANSCI'],
    user_lists => [
      'MANSCI',
      'admins',
      '[@maintenance=admins]',
    ],
  }
```

### MANSCI-GPU
```yaml
  sge::config::queue {'MANSCI-GPU':
    hostlist   => ['@MANSCI-GPU'],
    user_lists => [
      'MANSCI',
      'admins',
      '[@maintenance=admins]',
    ],
  }
```

## Updates to yaml
### BAIS
```yaml
# name: Qihang Lin
  # email: qihang-lin@uiowa.edu
  sge::config::queue {'BAIS':
    hostlist   => ['@BAIS'],
    user_lists => [
      'BAIS',
      'admins',
      '[@maintenance=admins]',
    ],
  }
```

### BAIS-GPU
```yaml
sge::config::queue {'BAIS-GPU':
    hostlist   => ['@BAIS-GPU'],
    user_lists => [
      'BAIS',
      'admins',
      '[@maintenance=admins]',
    ],
  }
```




Rename the queue

disable the queues 
change the names 
clush -a groupmod newname oldname
edit puppet
- ensure = abscent
move nodes
run puppt


clush -a groupmod newname oldname

