[Deutsch](00-scope.md) | [English](00-scope.en.md) | [Español](00-scope.es.md)

# Scope and Objectives Document — Mittelstand-IT-Lab

**Document:** 00 - Scope & Objectives  
**Version:** 1.0  
**Milestone:** M0 - Kickoff & Planning  
**Status:** Proposed  
**Author:** Ing. Daniel Alejandro Bolanos Solana  
**Date:** 2026-11-08  

## 1. Project Purpose

Mittelstand-IT-Lab is a personal infrastructure laboratory designed to demonstrate practical skills in Windows/Linux systems administration, nested virtualization, network segmentation and IT infrastructure security, with a focus on the real needs of the German mid-sized enterprise (Mittelstand).

The lab stems from **12 years operating critical infrastructures** (international airport, banking). It therefore prioritizes **verified recovery**, **least-privilege segmentation** and **auditable traceability**, not just a functional test environment.

## 2. Project Scope

### 2.1 In scope (v1)

| Component | Description |
| :--- | :--- |
| **Physical host (L0)** | Ubuntu 24.04 LTS with KVM/QEMU and libvirt as the virtualization layer. |
| **Platform (L1)** | Proxmox VE 8.2 deployed as a VM on the L0 layer. |
| **Edge (L2)** | OPNsense 24.1 as perimeter firewall and segmentation engine. |
| **Windows workloads** | Windows Server 2025 (DC01, DC02) with AD, DHCP and DNS roles. |
| **Linux workloads** | Debian 13 (MON01 for Zabbix, Client02 for critical testing). |
| **Clients** | Windows 11 (Client01) and Debian 13 (Client02). |
| **Network** | Segmentation via VLANs and planned IPv4 addressing (VLSM). |
| **Operations** | Monitoring (Zabbix) and backup strategy with recovery criteria. |

### 2.2 Out of scope (v1)

| Element | Reason for exclusion |
| :--- | :--- |
| Internet exposure of production services | The lab is for personal use; no services will be exposed to the Internet. |
| Multi-node high availability (HA) | The hardware is a single laptop; to be evaluated in v2. |
| Integration with cloud providers | Not necessary for the learning objectives. |
| Retail licenses for Windows Server | The official evaluation version will be used (180 days, renewable). |
| 24/7 support | The lab is not a production environment. |

## 3. Project Objectives

### 3.1 Technical Objectives

1.  Validate nested virtualization (KVM inside KVM) on an Ubuntu host.
2.  Deploy Proxmox VE as a level 1 hypervisor on the L0 layer.
3.  Implement a perimeter firewall (OPNsense) with VLAN segmentation.
4.  Deploy a Windows Server domain (Active Directory, DNS, DHCP).
5.  Integrate Linux servers (Debian) and Windows/Linux clients into the domain.
6.  Implement monitoring (Zabbix) and a verifiable backup strategy.

### 3.2 Learning Objectives

1.  Document every technical decision through ADRs (Architecture Decision Records).
2.  Maintain an atomic and descriptive commit history.
3.  Generate validation evidence in `docs/validation/` for each milestone.
4.  Apply the **Default-Deny** principle in the firewall policy.
5.  Practice Windows and Linux systems administration in a segmented environment.

## 4. Success Criteria

The project will be considered successful if, at the end of M5, the following criteria are met:

| Criterion | Evidence |
| :--- | :--- |
| The L0 host has KVM/QEMU operational and `/dev/kvm` accessible. | Screenshot of `egrep -c '(vmx|svm)' /proc/cpuinfo` and `virsh list`. |
| Proxmox VE is accessible via web and manages the VMs. | Screenshot of the Proxmox panel. |
| OPNsense segments traffic according to the flow matrix. | Connectivity tests (ping, traceroute) between VLANs. |
| The Windows Server domain is operational (AD, DNS, DHCP). | Screenshot of `Active Directory Users and Computers`. |
| Windows/Linux clients are joined to the domain. | Screenshot of `nslookup` and `ping` to the DC. |
| Zabbix monitors at least 3 VMs. | Screenshot of the Zabbix dashboard. |
| A verified backup has been restored. | Screenshot of the restore process and functionality test. |

## 5. Constraints and Assumptions

### 5.1 Constraints

- **Hardware:** A single laptop (Medion AVANTUN 15E1: AMD Ryzen 5 7430U, 32 GB RAM, 1 TB NVMe).
- **Budget:** Zero euros (all software is open-source or free evaluation).
- **Time:** The project is developed outside working hours (weekends).
- **Knowledge:** The author has 12 years of experience in critical infrastructures, but this is their first homelab with nested Proxmox.

### 5.2 Assumptions

- The hardware supports nested virtualization (AMD-V/SVM enabled in BIOS).
- The Windows Server 2025 evaluation version is sufficient for the lab.
- The physical router allows bridge creation without restrictions.
- The Internet connection is stable for downloading ISOs and updates.

## 6. Identified Risks

| Risk | Impact | Mitigation |
| :--- | :--- | :--- |
| Nested virtualization complexity | Medium | Validate KVM and `/dev/kvm` before deploying Proxmox (M1). |
| Segmentation errors | High | Document flow matrix, test per-VLAN connectivity, Default-Deny. |
| Configuration loss / drift | High | Versioned backup, snapshots, verified restore (M5). |
| Single point of failure (one host) | Medium | Conscious acceptance in v1; HA evaluation in v2. |
| Scope drift | Medium | Roadmap with closed acceptance criteria. |

## 7. Deliverables per Milestone

| Milestone | Deliverable |
| :--- | :--- |
| M0 | README, scope, architecture, ADRs, IP plan and flow matrix. |
| M1 | KVM/QEMU host operational and validated. |
| M2 | Proxmox VE deployed and accessible. |
| M3 | OPNsense configured with VLANs and firewall rules. |
| M4 | Windows Server domain and Linux/Windows clients integrated. |
| M5 | Monitoring (Zabbix) and verified backup. |

## 8. References

- [`README.en.md`](../README.en.md) — Main project document.
- [`docs/architecture/`](../architecture/) — Diagrams and network plans.
- [`docs/adr/`](../adr/) — Architecture decisions.
- [`docs/validation/`](../validation/) — Validation evidence.