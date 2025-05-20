




### **2. Check for Hidden VT-d Option**

Some Dell servers automatically enable VT-d with the "Virtualization Technology" option. To confirm, test IOMMU support:

#### **Check Kernel Boot Logs**

Run:

```sh
dmesg | grep DMAR
```

If you see lines like:
```sh
DMAR: IOMMU enabled

```

ex:
```sh
13:30:33 root @ pve-prod-coruscant-ssh_session in ~ 
❯ dmesg | grep DMAR
[    0.017603] ACPI: DMAR 0x00000000CD3346F4 0000E0 (v01 DELL   PE_SC3   00000001 DELL 00000001)
[    0.017679] ACPI: Reserving DMAR table memory at [mem 0xcd3346f4-0xcd3347d3]
[    0.128040] DMAR: IOMMU enabled
[    0.422693] DMAR: Host address width 46
[    0.422695] DMAR: DRHD base: 0x000000de100000 flags: 0x1
[    0.422713] DMAR: dmar0: reg_base_addr de100000 ver 1:0 cap d2078c106f0462 ecap f020fe
[    0.422717] DMAR: RMRR base: 0x000000cf458000 end: 0x000000cf46ffff
[    0.422725] DMAR: RMRR base: 0x000000cf450000 end: 0x000000cf450fff
[    0.422727] DMAR: RMRR base: 0x000000cf452000 end: 0x000000cf452fff
[    0.422729] DMAR: ATSR flags: 0x0
[    0.422736] DMAR-IR: IOAPIC id 0 under DRHD base  0xde100000 IOMMU 0
[    0.422739] DMAR-IR: IOAPIC id 1 under DRHD base  0xde100000 IOMMU 0
[    0.422741] DMAR-IR: HPET id 0 under DRHD base 0xde100000
[    0.422743] DMAR-IR: x2apic is disabled because BIOS sets x2apic opt out bit.
[    0.422744] DMAR-IR: Use 'intremap=no_x2apic_optout' to override the BIOS setting.
[    0.423167] DMAR-IR: Enabled IRQ remapping in xapic mode
[    0.675966] DMAR: No SATC found
[    0.675969] DMAR: dmar0: Using Queued invalidation
[    0.679656] DMAR: Intel(R) Virtualization Technology for Directed I/O

```