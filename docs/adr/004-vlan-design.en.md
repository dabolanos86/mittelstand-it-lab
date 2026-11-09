[Deutsch](004-vlan-design.md) | [English](004-vlan-design.en.md) | [Español](004-vlan-design.es.md)

# ADR 004 — VLAN Design and VLSM Addressing

- **Status:** Accepted
- **Milestone:** M0
- **Date:** 2026-11-05

## Context

The lab must host multiple workloads with different levels of criticality and trust: infrastructure servers (Windows Server, Debian), test clients (Windows 11, Debian), a perimeter firewall (OPNsense) and hypervisor management (Proxmox VE). Without segmentation, all traffic would share a single broadcast domain, which would increase the attack surface, hinder the application of security policies and make it impossible to simulate a real enterprise environment.

The available hardware is a single laptop with 32 GB of RAM, which limits the number of VMs and requires efficient use of IP addresses.

## Decision

A **VLAN-based segmentation** with **VLSM (Variable Length Subnet Mask)** addressing is implemented, assigning subnets sized to each zone according to its expected number of hosts. Segmentation is centrally managed on the firewall **FW01 (VM 500)**, which acts as the default gateway for all VLANs.

The defined VLANs are:

| VLAN | Name | Purpose | Subnet |
| :--- | :--- | :--- | :--- |
| 13 | Proxmox Management | L1 hypervisor management | `10.11.13.0/30` |
| 14 | Virtual Servers | Infrastructure servers | `10.11.14.0/29` |
| 7 | Virtual Normal Clients | Standard test clients | `10.11.7.0/28` |
| 3 | Virtual Critical Clients | Critical test clients | `10.11.3.0/29` |
| 17 | Admins | Administration network (Jump Host) | `10.11.17.0/30` |
| 21 | Firewall Management | Isolated firewall management | `10.11.21.0/30` |

## Alternatives considered

- **Flat network (no VLANs):**
  - *Advantage:* Trivial configuration, no need for a routing firewall.
  - *Disadvantage:* No isolation between zones; a compromised client could access servers or Proxmox management; does not represent a real enterprise environment; impossible to apply Default-Deny.
- **/24 subnets for all VLANs:**
  - *Advantage:* Easy to calculate and remember.
  - *Disadvantage:* Massive IP waste (254 hosts per VLAN when only 2-6 are needed); does not demonstrate subnetting skills; does not apply the least-privilege principle.
- **Physical segmentation (multiple switches/routers):**
  - *Advantage:* Real isolation at the physical level.
  - *Disadvantage:* Infeasible in a lab with a single laptop; costly; not scalable.

## Consequences

**Positive:**
- Real isolation between zones: a compromised client cannot access Proxmox management or critical servers.
- Application of the least-privilege principle: each VLAN only has the IPs it needs.
- Efficient use of address space (VLSM): IP waste is avoided.
- Simulation of a real enterprise environment: VLAN segmentation is the standard in the German mid-sized enterprise.
- Facilitates the application of the flow matrix (`flow-matrix.en.md`) with Default-Deny.

**Negative:**
- Greater configuration complexity: requires a firewall that routes between VLANs and a VLAN-Aware bridge on Proxmox.
- Requires careful IP addressing planning to avoid overlaps.
- Network troubleshooting is more complex (must verify routes, gateways and firewall rules).
- The laptop hardware limits the number of VMs, which in turn limits the number of hosts per VLAN.

## References

- [RFC 1918 — Address Allocation for Private Internets](https://datatracker.ietf.org/doc/html/rfc1918)
- [RFC 3022 — Traditional IP Network Address Translator (NAT)](https://datatracker.ietf.org/doc/html/rfc3022)
- [`docs/architecture/ip-plan.en.md`](../architecture/ip-plan.en.md) — Detailed IP addressing plan.
- [`docs/architecture/flow-matrix.en.md`](../architecture/flow-matrix.en.md) — Flow matrix that implements the segmentation.
- [`docs/architecture/vlan-plan-v1.png`](../architecture/vlan-plan-v1.png) — Visual diagram of the VLAN plan.