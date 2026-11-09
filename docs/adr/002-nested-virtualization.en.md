[Deutsch](002-nested-virtualization.de.md) | [English](002-nested-virtualization.en.md) | [Español](002-nested-virtualization.es.md)

# ADR 002 — Nested Virtualization: Proxmox VE on KVM/QEMU

- **Status:** Accepted
- **Milestone:** M0
- **Date:** 2026-11-01

## Context

The lab must host a level 1 hypervisor (Proxmox VE) to manage the project's virtual machines (Windows/Linux servers, OPNsense firewall, test clients). The available hardware is a single laptop (Medion AVANTUN 15E1: AMD Ryzen 5 7430U, 32 GB RAM, 1 TB NVMe), which is also used as a daily workstation with Ubuntu 24.04 LTS.

There are two main options for deploying Proxmox:

1.  **Bare-metal installation:** Install Proxmox VE directly on the physical disk, removing Ubuntu.
2.  **Nested virtualization:** Keep Ubuntu as the host, install KVM/QEMU + libvirt, and deploy Proxmox VE as a virtual machine (L1) on top of that layer.

## Decision

**Nested virtualization** is chosen. Proxmox VE 8.2 will be deployed as an L1 VM on Ubuntu 24.04 LTS (L0), using KVM/QEMU and libvirt as the virtualization engine. The Proxmox VM will be assigned 4 vCPUs, 16 GB RAM and 300 GB of disk, and will be connected to the `vmbr0` bridge to have access to the WAN and internal VLANs.

## Alternatives considered

- **Bare-metal (Proxmox directly on the hardware):**
  - *Advantage:* Maximum performance, no intermediate layer.
  - *Disadvantage:* Removes Ubuntu as the daily work environment; any Proxmox failure makes the laptop unusable for other tasks; difficult to revert without reinstalling the OS.
- **VirtualBox / VMware Workstation:**
  - *Advantage:* Easy to use.
  - *Disadvantage:* Lower performance, does not natively support KVM nesting, not representative of an enterprise environment.
- **Containers (LXC/Docker):**
  - *Advantage:* Lightweight.
  - *Disadvantage:* Does not allow simulating a level 1 hypervisor or testing full virtualization scenarios (Windows Server, OPNsense).

## Consequences

**Positive:**
- Ubuntu is preserved as the daily work environment (IDE, browser, tools).
- The laptop remains usable even if Proxmox fails; it can be restored from an L1 VM snapshot.
- Nested virtualization is practiced, a skill in demand in cloud environments and test labs.
- Allows taking snapshots of the hypervisor itself before risky changes.
- Isolates the lab from the host operating system.

**Negative:**
- Performance penalty due to the double virtualization layer (estimated at 10-15%).
- Additional complexity in network configuration (bridges, VLAN-Aware bridges).
- The laptop hardware (32 GB RAM) limits the number of VMs that can run simultaneously.
- Nested virtualization requires AMD-V (SVM Mode) to be enabled in the BIOS.

## References

- [Proxmox VE Documentation — Nested Virtualization](https://pve.proxmox.com/wiki/Nested_Virtualization)
- [KVM — Nested Guests](https://www.linux-kvm.org/page/Nested_Guests)
- `docs/01-host-kvm-audit.md` — *(Pending creation in M1)* Evidence of `/dev/kvm` validation on the L0 host.