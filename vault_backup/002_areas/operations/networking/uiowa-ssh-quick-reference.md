---
title: "uiowa-ssh-quick-reference"
created: 2024-10-28
modified: 2024-10-28
type: reference
system: hpc
tags:
  - hpc
  - ssh
  - idas
  - lss
  - argon
---

## RS Infrastructure SSH Quick Reference

### IDAS

**Primary Utility Node**
```shell
ssh rs-k8-util02.hpc.uiowa.edu
```

**Compute Node Access**
```shell
ssh rs-k8-compute01.idas
```

---

### LSS

**LSS Host Access**
```shell
ssh <hostName>.hpc.uiowa.edu
```

---

### Argon

**Argon Log in node** - Round robin DNS
```shell
ssh argon.hpc.uiowa.edu
```

**Argon Head Node Access**
```shell
ssh argon-itf-head.hpc.uiowa.edu
```

**SSH Dynamic Port Forwarding with SOCKS Proxy**
```shell
ssh -D 1080 argon-itf-head.hpc.uiowa.edu
```

```sh
ssh -D 9090 rs-jump.its.uiowa.edu
```


> **Note**: This command creates an SSH connection to `argon-itf-head.hpc.uiowa.edu` with a SOCKS proxy on port 1080, allowing applications to route traffic through the SSH tunnel.

To access IPMI via Firefox, use:
```plaintext
https://<hostName>.ipmi
```


### iihg

```sh
ssh jump.iihg.uiowa.edu
```

```sh
ssh itf-iihg-stor01.iihg.uiowa.edu
```

- or it will be `.morl` instead of `.iihg`