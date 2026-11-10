[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

# Logical and Network Architecture — Mittelstand-IT-Lab

**Document:** Architecture Index  
**Version:** 1.0  
**Milestone:** M0 - Kickoff & Planning  
**Status:** Proposed  
**Date:** 2026-11-10  

## 📌 Purpose of this directory

This directory contains the logical design artifacts of the lab infrastructure. Here we define **how** the system is built (virtualization layers), **how** the components communicate (network topology) and **which** IP addresses and VLANs will be used.

All documentation here represents the **"As-Designed"** state. The evidence that the system works as designed will be found in `docs/validation/` once the implementation milestones (M1 onwards) are executed.

## 🗂️ Artifact Index

The documents and diagrams available in this folder are detailed below:

| Artifact | Type | Description |
| :--- | :--- | :--- |
| [`architecture-v1.png`](./architecture-v1.png) | Diagram | Hierarchical view of the infrastructure (L0 Physical Host, L1 Nested Proxmox, L2 VMs). |
| [`architecture-v1.drawio`](./architecture-v1.drawio) | Source | Editable file of the hierarchical diagram. |
| [`vlan-plan-v1.png`](./vlan-plan-v1.png) | Diagram | Visual table of the VLAN plan, subnets (VLSM) and Gateways. |
| [`vlan-plan-v1.drawio`](./vlan-plan-v1.drawio) | Source | Editable file of the VLAN plan. |
| [`ip-plan.en.md`](./ip-plan.en.md) | Document | Detailed IP addressing plan, VM inventory and Firewall interfaces. |
| [`flow-matrix.en.md`](./flow-matrix.en.md) | Document | Firewall flow matrix (Default-Deny and exceptions). |

## 🔗 Relationship between Artifacts

*   The **Architecture Diagram** (`architecture-v1.png`) shows the physical and virtual hierarchy.
*   The **VLAN Plan** (`vlan-plan-v1.png`) defines L2 segmentation.
*   The **IP Plan** (`ip-plan.en.md`) translates that segmentation into concrete addresses for each VM.
*   The **Flow Matrix** (`flow-matrix.en.md`) dictates which traffic can cross between those IPs through the Firewall.

## 🛠️ Editing Tools

Files with the `.drawio` extension have been created with [Draw.io], the `.md` files have been created with [Ghostwriter], both on an Ubuntu operating system. To modify them, it is recommended to open them with these tools (web or desktop) and, in the case of `.drawio` files, export the result in high-resolution `.png` format to maintain visual coherence.