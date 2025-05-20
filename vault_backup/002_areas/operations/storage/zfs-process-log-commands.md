---
title: zfs-process-log-commands
created: 2024-12-30
type: command-reference
system: 
tags:
  - commands
  - uiowa
  - zfs
  - troubleshooting
  - process
related_runbooks:
---
# Commands


## Process Investigation

Listing number of process

```sh
ps -ef | grep zfs | wc -l
```




Listing process in order
```sh
ps -eo pid,lstart,cmd | grep zfs
```

Example output showing wedged processes
```sh
[hawrth@itf-rs-store31 ~]$ ps -eo pid,lstart,cmd | grep zfs
  8669 Tue Jan 28 18:03:43 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/busserv_archive
  8801 Tue Jan 28 18:03:43 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/busserv_archive
 10051 Tue Jan 28 18:01:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_Provost_HRShared
 11594 Tue Jan 28 18:01:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_Provost_HRShared
 11657 Tue Jan 28 18:01:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_PBS_Shop
 11658 Tue Jan 28 18:01:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_psychhomes
 11659 Tue Jan 28 18:01:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_willour
 12533 Tue Jan 28 18:01:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_lss_TRP4
 12936 Tue Jan 28 18:01:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_willour
 12942 Tue Jan 28 18:01:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_psychhomes
 12943 Tue Jan 28 18:01:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_PBS_Shop
 13452 Tue Jan 28 18:01:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_lss_TRP4
 16640 Tue Jan 28 18:14:59 2025 sudo zfs snapshot dpool01/Shared/lss_rstest_itf-rs-store31@2025-01-28-181500-snapshot
 16641 Tue Jan 28 18:14:59 2025 sudo zfs snapshot dpool01/Shared/neon_admin_archive@2025-01-28-181500-snapshot
 16820 Tue Jan 28 18:14:59 2025 zfs snapshot dpool01/Shared/lss_rstest_itf-rs-store31@2025-01-28-181500-snapshot
 16824 Tue Jan 28 18:14:59 2025 zfs snapshot dpool01/Shared/neon_admin_archive@2025-01-28-181500-snapshot
 16899 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/lss_ymeurice
 16900 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/meerdink
 16901 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/UHDStorage
 16902 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/IWP
 16903 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/lss_ldecastro
 16904 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/Fernapogamy
 16905 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/GSLA
 16906 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/honorsarchive
 16907 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/lss_mjmcgill
 16908 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/Bates_Tomasson_Core_Data
 16909 Tue Jan 28 18:03:44 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/hancher_archive
 16910 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/lss_ymeurice
 16911 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/UHDStorage
 16912 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/meerdink
 16913 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/honorsarchive
 16914 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/lss_ldecastro
 16915 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/IWP
 16916 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/Bates_Tomasson_Core_Data
 16917 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/lss_mjmcgill
 16918 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/Fernapogamy
 16919 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/GSLA
 16920 Tue Jan 28 18:03:44 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Shared/hancher_archive
 17737 Tue Jan 28 18:14:59 2025 sudo zfs snapshot dpool01/Shared/argon_admin_archive@2025-01-28-181500-snapshot
 17818 Tue Jan 28 18:14:59 2025 zfs snapshot dpool01/Shared/argon_admin_archive@2025-01-28-181500-snapshot
 18582 Tue Jan 28 18:01:45 2025 sudo zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_lss_IntMed
 18672 Tue Jan 28 18:01:45 2025 zfs list -H -o name -t snapshot -s creation -d 1 dpool01/Backups/zfs/lc-rs-storage20.hpc.uiowa.edu/dpool01_Shared_lss_IntMed
 32116 Tue Jan 28 18:03:46 2025 sh -c sudo zfs send -I dpool01/Shared/lss_hvergaraarrieta@2024-10-19-190000-snapshot dpool01/Shared/lss_hvergaraarrieta@2025-01-28-180000-snapshot|ssh -o batchMode=yes -o ConnectTimeout=30 'lss_hvergaraarrieta_bak@lc-rs-store24.nfs' 'sudo zfs recv -F '"'"'dpool01/Backups/zfs/itf-rs-store31.hpc.uiowa.edu/dpool01_Shared_lss_hvergaraarrieta'"'"''
 32143 Tue Jan 28 18:03:46 2025 sudo zfs send -I dpool01/Shared/lss_hvergaraarrieta@2024-10-19-190000-snapshot dpool01/Shared/lss_hvergaraarrieta@2025-01-28-180000-snapshot
 32144 Tue Jan 28 18:03:46 2025 ssh -o batchMode=yes -o ConnectTimeout=30 lss_hvergaraarrieta_bak@lc-rs-store24.nfs sudo zfs recv -F 'dpool01/Backups/zfs/itf-rs-store31.hpc.uiowa.edu/dpool01_Shared_lss_hvergaraarrieta'
 32198 Tue Jan 28 18:03:46 2025 zfs send -I dpool01/Shared/lss_hvergaraarrieta@2024-10-19-190000-snapshot dpool01/Shared/lss_hvergaraarrieta@2025-01-28-180000-snapshot
```




---
```sh
sudo dmesg --ctime | grep -Ei 'error|failed|failure'
```

```sh
sudo dmesg --ctime | grep --color=always -Ei 'scsi|ata|sd|nvme|fault|error|fail|failed' | less -R
```




```sh
dmesg -HL | grep -Ei 'error|failed|failure'

```

Example Output to help identify disk or issue
```sh
[Jan28 14:33] sd 0:0:21:0: [sdv] tag#0 FAILED Result: hostbyte=DID_OK driverbyte=DRIVER_SENSE cmd_age=2s
[  +0.000016] sd 0:0:21:0: [sdv] tag#0 Sense Key : Medium Error [current] [descriptor]
[  +0.000006] sd 0:0:21:0: [sdv] tag#0 Add. Sense: Unrecovered read error
[  +0.000006] blk_update_request: critical medium error, dev sdv, sector 35156509192
[  +2.906878] sd 0:0:21:0: [sdv] tag#13 FAILED Result: hostbyte=DID_OK driverbyte=DRIVER_SENSE cmd_age=2s
[  +0.000023] sd 0:0:21:0: [sdv] tag#13 Sense Key : Medium Error [current] [descriptor]
[  +0.000008] sd 0:0:21:0: [sdv] tag#13 Add. Sense: Unrecovered read error
[  +0.000007] blk_update_request: critical medium error, dev sdv, sector 35156509192
[  +0.001438] Buffer I/O error on dev sdv9, logical block 65, async page read
```





