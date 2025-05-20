---
title: idas-labeling-node
created: 2024-11-26
type: process
system: 
tags:
  - process
  - procedure
  - uiowa
related_docs:
---
   **Modified:**
Adding labels to host

kube using node labels make sure jobs land on correct host there are three

- compute - cosmetic
- Jupyter role - what idas use assign jobs to the right node
- classroom role
- gpuType

```sh
hawrth@rs-k8-util02:~$ kubectl get nodes
NAME              STATUS   ROLES           AGE      VERSION
rs-k8-app01       Ready    app             187d     v1.29.5
rs-k8-app02       Ready    app             187d     v1.29.5
rs-k8-compute01   Ready    compute         187d     v1.29.5
rs-k8-compute02   Ready    compute         187d     v1.29.5
rs-k8-compute03   Ready    compute         187d     v1.29.5
rs-k8-compute04   Ready    compute         185d     v1.29.5
rs-k8-compute05   Ready    compute         187d     v1.29.5
rs-k8-compute06   Ready    compute         187d     v1.29.5
rs-k8-compute07   Ready    compute         187d     v1.29.5
rs-k8-compute08   Ready    compute         187d     v1.29.5
rs-k8-compute09   Ready    compute         187d     v1.29.5
rs-k8-compute10   Ready    compute         187d     v1.29.5
rs-k8-compute11   Ready    compute         187d     v1.29.5
rs-k8-compute12   Ready    compute         187d     v1.29.5
rs-k8-compute13   Ready    compute         10d      v1.29.5
rs-k8-compute14   Ready    <none>          5d21h    v1.29.5
rs-k8-head01      Ready    control-plane   2y333d   v1.29.5
rs-k8-head02      Ready    control-plane   2y333d   v1.29.5
rs-k8-util03      Ready    util            187d     v1.29.10
hawrth@rs-k8-util02:~$
kubectl label node rs-k8-compute14 node-role.kubernetes.io/compute=
node/rs-k8-compute14 labeled
hawrth@rs-k8-util02:~$ kubectl get nodes
NAME              STATUS   ROLES           AGE      VERSION
rs-k8-app01       Ready    app             187d     v1.29.5
rs-k8-app02       Ready    app             187d     v1.29.5
rs-k8-compute01   Ready    compute         187d     v1.29.5
rs-k8-compute02   Ready    compute         187d     v1.29.5
rs-k8-compute03   Ready    compute         187d     v1.29.5
rs-k8-compute04   Ready    compute         185d     v1.29.5
rs-k8-compute05   Ready    compute         187d     v1.29.5
rs-k8-compute06   Ready    compute         187d     v1.29.5
rs-k8-compute07   Ready    compute         187d     v1.29.5
rs-k8-compute08   Ready    compute         187d     v1.29.5
rs-k8-compute09   Ready    compute         187d     v1.29.5
rs-k8-compute10   Ready    compute         187d     v1.29.5
rs-k8-compute11   Ready    compute         187d     v1.29.5
rs-k8-compute12   Ready    compute         187d     v1.29.5
rs-k8-compute13   Ready    compute         10d      v1.29.5
rs-k8-compute14   Ready    compute         5d21h    v1.29.5
rs-k8-head01      Ready    control-plane   2y333d   v1.29.5
rs-k8-head02      Ready    control-plane   2y333d   v1.29.5
rs-k8-util03      Ready    util            187d     v1.29.10
hawrth@rs-k8-util02:~$ kubectl label node rs-k8-compute13 jupyterRole=compute
node/rs-k8-compute13 labeled
hawrth@rs-k8-util02:~$ kubectl label node rs-k8-compute14 jupyterRole=compute
node/rs-k8-compute14 labeled
hawrth@rs-k8-util02:~$ kubectl label node rs-k8-compute13 gpuType=L40S
node/rs-k8-compute13 labeled
hawrth@rs-k8-util02:~$ kubectl label node rs-k8-compute14 gpuType=L40S
node/rs-k8-compute14 labeled
hawrth@rs-k8-util02:~$
```



```sh
hawrth@rs-k8-util02:~$ kubectl label node rs-k8-compute13 jupyterRole=compute
node/rs-k8-compute13 labeled
hawrth@rs-k8-util02:~$ kubectl label node rs-k8-compute14 jupyterRole=compute
node/rs-k8-compute14 labeled
hawrth@rs-k8-util02:~$ kubectl label node rs-k8-compute13 gpuType=L40S
node/rs-k8-compute13 labeled
hawrth@rs-k8-util02:~$ kubectl label node rs-k8-compute14 gpuType=L40S
node/rs-k8-compute14 labeled
hawrth@rs-k8-util02:~$
```



---





enabled l40s in db

Database name is **`ITS_RS_IDAS`** on **`iowasqlhost5`**

```sql
INSERT INTO gpu_type (gpu_name, enabled) VALUES ('L40S', 1) 
```



