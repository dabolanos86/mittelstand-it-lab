[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

# Project Documentation — Mittelstand-IT-Lab

**Document:** General Documentation Index  
**Version:** 1.0  
**Milestone:** M0 - Kickoff & Planning  
**Status:** Proposed  
**Date:** 2026-11-10  

## 📌 Purpose of this directory

This folder contains the project's technical documentation, organized by milestones and by artifact type. It records the complete lifecycle: planning (M0), implementation (M1-M2), segmentation (M3), workloads (M4) and operations (M5).

## 🗂️ General Index

### Cross-cutting Documents

| Document | Description | Languages |
| :--- | :--- | :--- |
| [`00-scope.en.md`](./00-scope.en.md) | Project scope, objectives, success criteria and risks. | [ES](./00-scope.es.md) · [EN](./00-scope.en.md) · [DE](./00-scope.md) |

### Architecture and Network

| Document | Description | Languages |
| :--- | :--- | :--- |
| [`architecture/`](./architecture/) | Diagrams, IP plan and flow matrix. | [ES](./architecture/README.es.md) · [EN](./architecture/README.en.md) · [DE](./architecture/README.md) |

### Architecture Decision Records (ADRs)

| Document | Description | Languages |
| :--- | :--- | :--- |
| [`adr/`](./adr/) | Record of technical and process decisions. | [ES](./adr/README.es.md) · [EN](./adr/README.en.md) · [DE](./adr/README.md) |

### Documentation per Milestone

| Milestone | Document | Status |
| :--- | :--- | :--- |
| M0 | `00-scope.es.md` | ✅ Completed |
| M1 | `01-host-kvm-audit.md` | ⬜ Pending |
| M2 | `02-proxmox-deployment.md` | ⬜ Pending |
| M3 | `03-opnsense-segmentation.md` | ⬜ Pending |
| M4 | `04-workloads-deployment.md` | ⬜ Pending |
| M5 | `05-monitoring-backup.md` | ⬜ Pending |

### Validation Evidence

| Folder | Description |
| :--- | :--- |
| [`validation/`](./validation/) | Screenshots, command outputs and test results. |

## 🔗 Relationship with other documents

- **`README.en.md` (root)**: Main project document (project charter).
- **`docs/architecture/`**: Logical and network design.
- **`docs/adr/`**: Architecture and process decisions.
- **`docs/validation/`**: Evidence that the system works as designed.

## 🛠️ Conventions

- **Numbering per milestone:** `NN-descriptive-title.md` (e.g. `01-host-kvm-audit.md`).
- **Languages:** Each document is published in Spanish (source), English and German.
- **Status:** `Proposed`, `Accepted`, `In progress`, `Completed`.