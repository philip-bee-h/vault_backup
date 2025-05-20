---
title: "puppet-command-reference"
created: 2024-10-28
modified: 2024-10-28
type: command-reference
system: infrastructure
tags:
  - puppet
  - alias
  - commands
  - infrastructure
---
sd
## Description
This is a quick reference guide for the aliases I use for puppet on my work machine.

## Commands

**Puppet log in**
```shell
puppet-access login --lifetime=4h hawrth
```

**Deploy**
```shell
puppet-code deploy <repo> --wait
```

## Aliases

### Puppet Login
```shell
alias pelogin='puppet-access login --lifetime=4h hawrth'
```

```shell
pelogin
```

### Deploying puppet code
```shell
alias pedeploy='puppet-code deploy'
```
**Command**
```shell
pedeploy <repo> --wait
```

> [!tip] 
> Make sure to replace **repo** with the correct repo


