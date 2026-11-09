[Deutsch](ip-plan.md) | [English](ip-plan.en.md) | [Español](ip-plan.es.md)

# IP Addressing Plan and Logical Topology

**Project:** Mittelstand-IT-Lab  
**Document:** IP Addressing Plan & Network Topology  
**Version:** 1.0  
**Milestone:** M0 - Kickoff & Planning  
**Status:** Proposed  
**Date:** 2026-11-05  

## 1. Executive Summary
This document defines the IPv4 addressing assignment, VLAN segmentation and logical topology of the virtualized infrastructure. The design applies the **least-privilege** principle through the use of VLSM (Variable Length Subnet Mask), strictly separating management traffic, servers, normal clients, critical clients and the administration network.

All internal segmentation is managed and routed by the virtual machine **FW01 (VM 500)**, which acts as perimeter firewall and default gateway for all internal subnets.

## 2. Network and VLAN Table (L2/L3)

| Network / Description | VLAN | Subnet / Base IP | Mask | Gateway (FW01) | Notes / Purpose |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **WAN / Internet** | *N/A* | `192.168.1.0/24` | `255.255.255.0` | `192.168.1.1` | Home physical network (ISP Router). Firewall WAN interface: `.50` |
| **Firewall Management**| 21 | `10.11.21.0/30` | `255.255.255.252` | `10.11.21.1` | Isolated Firewall management (FW01). |
| **Proxmox Management** | 13 | `10.11.13.0/30` | `255.255.255.252` | `10.11.13.1` | L1 hypervisor management interface (Proxmox VE). IP: `.2` |
| **Virtual Servers** | 14 | `10.11.14.0/29` | `255.255.255.248` | `10.11.14.1` | Infrastructure servers (DC01, DC02, MON01). |
| **Virtual Normal Clients** | 7 | `10.11.7.0/28` | `255.255.255.240` | `10.11.7.1` | Standard test clients (Client01). |
| **Virtual Critical Clients**| 3 | `10.11.3.0/29` | `255.255.255.248` | `10.11.3.1` | Critical test clients / future services (Client02). |
| **Admins** | 17 | `10.11.17.0/30` | `255.255.255.252` | `10.11.17.1` | Network for Jump Host / SSH/RDP management from the physical host. |

## 3. Virtual Machine Inventory (L2) and IP Assignment

| VM ID | Hostname | Role | OS | VLAN | Assigned IP | Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **VM 100** | `DC01` | AD, DHCP, DNS (Primary) | Windows Server 2025 | 14 | `10.11.14.10` | `10.11.14.1` |
| **VM 101** | `DC02` | AD, DHCP, DNS (Secondary) | Windows Server 2025 | 14 | `10.11.14.11` | `10.11.14.1` |
| **VM 102** | `MON01` | Monitoring (Zabbix) | Debian 13 | 14 | `10.11.14.13` | `10.11.14.1` |
| **VM 200** | `Client01` | Standard client (Testing) | Windows 11 | 7 | `10.11.7.5` | `10.11.7.1` |
| **VM 201** | `Client02` | Critical client (Testing) | Debian 13 | 3 | `10.11.3.5` | `10.11.3.1` |
| **VM 500** | `FW01` | Perimeter Firewall / Router | OPNsense 26.7 | 21, 13, 14, 7, 3 | See interface note | *N/A* |

## 4. Firewall Interface Topology (VM 500)

The Firewall VM (`FW01`) is the central routing node. Its virtual interfaces (vNICs) will be configured as follows:

| Virtual Interface | Type | VLAN | IP / Mask | Description |
| :--- | :--- | :--- | :--- | :--- |
| `vtnet0` / `em0` | WAN | *N/A* | `192.168.1.50/24` | Internet uplink (Bridge `vmbr0` on L0 Host). |
| `vtnet1` / `em1` | LAN | 14 | `10.11.14.1/29` | Servers Gateway (DC01, DC02, MON01). |
| `vtnet2` / `em2` | LAN | 7 | `10.11.7.1/28` | Normal Clients Gateway. |
| `vtnet3` / `em3` | LAN | 3 | `10.11.3.1/29` | Critical Clients Gateway. |
| `vtnet4` / `em4` | LAN | 13 | `10.11.13.1/30` | Proxmox Management Gateway. |
| `vtnet5` / `em5` | LAN | 21 | `10.11.21.1/30` | Firewall's own Management Interface. |
| `vtnet6` / `em6` | LAN | 17 | `10.11.17.1/30` | Administration Gateway. |

## 5. Implementation Considerations (M1 / M2)

1.  **Bridge on the Physical Host (L0):** The `/etc/netplan/00-installer-config.yaml` file on the Ubuntu host must configure a bridge (`vmbr0`) with the IP `192.168.1.37/24` to provide the Firewall WAN uplink and allow host access.
2.  **VLAN-Aware Bridge on Proxmox (L1):** The internal Proxmox bridge must be configured as *VLAN-Aware* so the Firewall can tag traffic for the different subnets (14, 7, 3, etc.) without needing to create a physical bridge per VLAN.
3.  **Default-Deny:** The initial firewall policy (`FW01`) on all LAN interfaces will be to **deny all traffic** and then apply the exceptions documented in the `flow-matrix.md` file.
4.  **WAN IP Reservation:** The IP `192.168.1.50` must be reserved outside the physical router's DHCP pool (ISP) to guarantee the Firewall always receives the same address on the WAN.