# Mittelstand-IT-Lab

[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

**Nested Virtual Infrastructure (Proxmox VE), Systems Administration (Windows/Linux) and Network Segmentation**

> **Status: M0 completed — Kickoff and planning.** This README is the project's front page. The complete detail of scope, objectives, success criteria and risks is in [`docs/00-scope.en.md`](docs/00-scope.en.md). Implementation is recorded incrementally in the commit history and in `docs/`.

## 1. Purpose and professional context

Mittelstand-IT-Lab is a personal infrastructure laboratory designed to demonstrate practical skills in Windows/Linux systems administration, nested virtualization, network segmentation and IT infrastructure security, with a focus on the real needs of the German mid-sized enterprise (Mittelstand).

The lab stems from **12 years operating critical infrastructures** (international airport, banking). It therefore prioritizes **verified recovery**, **least-privilege segmentation** and **auditable traceability**, not just a functional test environment.

The complete scope, technical and learning objectives, success criteria, constraints, assumptions and risks are documented in [`docs/00-scope.en.md`](docs/00-scope.en.md).

## 2. Target architecture

| Layer | Component | Function |
|---|---|---|
| Host (L0) | Ubuntu 26.04.1 LTS | Host system, virtualization base |
| Virtualization | KVM/QEMU + libvirt | Virtualization engine on host |
| Platform (L1) | Proxmox VE 9.2-1 (VM) | VM, storage and network management |
| Edge (L2) | OPNsense 26.7 | Firewall, NAT, VLAN segmentation |
| Workloads | Debian 13 / Windows Server 2025 | Services and administration tests |
| Operations | Monitoring + Backup | Observability and recovery |

The architecture diagram is in [`docs/architecture/architecture-v1.png`](docs/architecture/architecture-v1.png). The IP plan and flow matrix are in [`docs/architecture/`](docs/architecture/).

## 3. Roadmap summary

| Milestone | Deliverable | Status |
|---|---|---|
| M0 | README, scope, architecture and ADRs | ✅ Completed |
| M1 | KVM/QEMU host operational and validated | ⬜ Pending |
| M2 | Proxmox VE deployed and accessible | ⬜ Pending |
| M3 | OPNsense + VLAN segmentation | ⬜ Pending |
| M4 | Linux/Windows workloads integrated | ⬜ Pending |
| M5 | Monitoring and verified backup | ⬜ Pending |

The detailed acceptance criteria for each milestone are in [`docs/00-scope.en.md`](docs/00-scope.en.md) (section 4).

## 4. Traceability model

- **Commit convention:** `type(scope): description` — e.g. `docs(architecture): add network topology v1`.
- **Documentation per phase:** each milestone generates a document in `docs/NN-phase.md`.
- **Decision log:** relevant technical decisions are recorded as ADRs in [`docs/adr/`](docs/adr/).
- **Requirement → implementation → validation traceability:** each scope requirement references its milestone and validation evidence in [`docs/validation/`](docs/validation/).
- **History as evidence:** the commit history is the source of truth for project progress.

## 5. Documentation and evidence

Project documentation is maintained in `docs/`:
- [`docs/00-scope.en.md`](docs/00-scope.en.md) — Scope, objectives, success criteria, risks.
- [`docs/architecture/`](docs/architecture/) — Diagrams, IP plan and flow matrix.
- [`docs/adr/`](docs/adr/) — Architecture and process decision records.
- [`docs/validation/`](docs/validation/) — Validation evidence (screenshots, command outputs, test results).

## 6. Notes on language and authorship

- **Source of authorship:** Spanish (`README.es.md`). The technical content (architecture, acceptance criteria, validation evidence) is original work.
- **Authoritative publication version:** German (`README.md`), oriented to the target market (German Mittelstand).
- **Secondary version:** English (`README.en.md`).
- **Translation:** DE and EN are generated with AI assistance and manually reviewed. AI is used exclusively for the linguistic version, not for technical content.
- **Synchronization:** on content changes, the Spanish source is updated first and then DE/EN are synchronized (ES → DE/EN).
- Decision recorded in [`docs/adr/001-ai-translation.en.md`](docs/adr/001-ai-translation.en.md).

## 7. Project status

**Current phase: M0 completed — Kickoff and planning. M1 in progress.**
The infrastructure is not yet fully deployed. Architecture design, network planning, implementation and testing will be documented as the project progresses, maintaining full traceability from this initial commit. The first validation evidence will be published in [`docs/validation/`](docs/validation/) when M1 is closed.