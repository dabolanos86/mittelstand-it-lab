[Deutsch](flow-matrix.md) | [English](flow-matrix.en.md) | [Español](flow-matrix.es.md)

# Network Flow Matrix and Firewall Policy

**Project:** Mittelstand-IT-Lab  
**Document:** Network Flow Matrix & Firewall Policy  
**Version:** 1.0  
**Milestone:** M0 - Kickoff & Planning  
**Status:** Proposed  
**Date:** 2026-11-05  

## 1. Executive Summary

This document defines the network security policy of the lab, establishing which traffic is allowed and which must be blocked between the different VLANs and towards the Internet. The policy is based on the **Default-Deny** principle: all traffic is blocked unless an explicit rule allows it.

The perimeter firewall **FW01 (VM 500)** is the only control point between the trusted zones (servers, clients, management) and the untrusted zone (WAN/Internet). No internal service is exposed directly to the Internet without passing through the firewall's NAT and filtering rules.

## 2. Security Philosophy (Zero Trust)

Network segmentation is based on the **Zero Trust** model, where no zone trusts another by default. Every traffic flow must be justified, documented and explicitly approved.

| Zone | VLAN | Trust Level | Description |
| :--- | :--- | :--- | :--- |
| **WAN / Internet** | *N/A* | Untrusted | Home physical network (ISP Router). |
| **Admins** | 17 | Maximum | Administration network (Jump Host). |
| **Proxmox Management** | 13 | High | L1 hypervisor management. |
| **Firewall Management** | 21 | High | Isolated firewall management. |
| **Virtual Servers** | 14 | Medium | Infrastructure servers (AD, DNS, Zabbix). |
| **Virtual Critical Clients** | 3 | Medium-Low | Critical test clients. |
| **Virtual Normal Clients** | 7 | Low | Standard test clients. |

## 3. Allowed Flow Matrix (Firewall Rules)

The following table details the **exceptions** to the Default-Deny policy. Any flow not listed here will be dropped by the firewall (Drop/Reject).

| # | Source | Destination | Port / Protocol | Action | Justification / Purpose |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | **VLAN 7** (Normal Clients) | **VLAN 14** (Servers) | TCP/UDP 53 (DNS) | **Allow** | DNS name resolution through DC01/DC02. |
| 2 | **VLAN 7** (Normal Clients) | **VLAN 14** (Servers) | TCP 88 (Kerberos), TCP 389 (LDAP) | **Allow** | Domain authentication (Active Directory). |
| 3 | **VLAN 7** (Normal Clients) | **VLAN 14** (Servers) | TCP 445 (SMB) | **Allow** | Access to file shares (specific servers only). |
| 4 | **VLAN 14** (Servers) | **WAN** (Internet) | TCP 443 (HTTPS), TCP 80 (HTTP) | **Allow** | Operating system updates and security patches. |
| 5 | **VLAN 14** (Servers) | **WAN** (Internet) | TCP/UDP 53 (DNS) | **Allow** | External DNS resolution (only if DCs do not cover everything). |
| 6 | **VLAN 17** (Admins) | **All VLANs** | TCP 22 (SSH), TCP 3389 (RDP), TCP 443 (HTTPS) | **Allow** | Administrative access to servers, VMs and Proxmox. |
| 7 | **VLAN 13** (Proxmox Mgmt) | **WAN** (Internet) | TCP 443 (HTTPS) | **Allow** | Access to the Proxmox web interface from the home network (static IP only). |
| 8 | **VLAN 3** (Critical Clients) | **WAN** (Internet) | TCP 443 (HTTPS) | **Allow** | Critical client updates (Debian). |
| 9 | **WAN** (Internet) | **VM 500 (FW01)** | TCP 443 (HTTPS), UDP 51820 (WireGuard) | **Allow** | Remote access to firewall management (VPN) and web interface. |

## 4. Denied Flow Matrix (Explicit Isolation Rules)

To prevent information leaks and lateral movement in case of compromise, the following explicit blocking rules will be applied (in addition to the global Default-Deny):

| Source | Destination | Action | Reason for Blocking |
| :--- | :--- | :--- | :--- |
| **VLAN 7** (Normal Clients) | **VLAN 13** (Proxmox Mgmt) | **Deny** | Hypervisor isolation: clients must not see or touch Proxmox management. |
| **VLAN 7** (Normal Clients) | **VLAN 3** (Critical Clients) | **Deny** | Zone isolation: normal clients must not communicate with critical ones. |
| **VLAN 3** (Critical Clients) | **VLAN 7** (Normal Clients) | **Deny** | Zone isolation: critical clients do not initiate connections to the user network. |
| **VLAN 7** (Normal Clients) | **WAN** (Internet) | **Deny** | Test clients have NO direct Internet access. Everything goes through the DC. |
| **VLAN 14** (Servers) | **VLAN 7 / VLAN 3** | **Deny** | Servers do not initiate connections to clients (they only respond). |
| **WAN** (Internet) | **VLAN 13, 14, 7, 3** | **Deny** | No internal service should be exposed directly to the Internet (without NAT/Port Forward). |

## 5. Implementation Considerations (M3)

1.  **NAT (Network Address Translation):** The firewall (`FW01`) will apply source NAT (SNAT) for all traffic going from internal VLANs to the WAN. Internal IPs (`10.11.x.x`) will never be visible on the Internet.
2.  **Logging:** All **Deny** rules must have logging enabled to audit intrusion attempts or misconfigurations. Allow rules will only be logged if strictly necessary (to avoid filling the firewall's disks).
3.  **Maintenance:** This matrix will be reviewed at the end of each milestone (M3, M4, M5). If a new VM or service is added, this document must be updated **before** touching the firewall configuration.
4.  **Aliases and Groups:** In OPNsense, it is recommended to create **Aliases** (e.g. `NET_SERVERS`, `NET_CLIENTS`) and **Groups** to simplify the rules. Instead of writing one rule per IP, aliases are used, which facilitates maintenance and reduces errors.
5.  **Remote Access VPN:** Rule 9 assumes a WireGuard VPN for remote access. If it is not implemented in v1, this rule must be removed or disabled until the VPN is configured.