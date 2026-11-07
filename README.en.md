# Mittelstand-IT-Lab

[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

**Nested Virtual Infrastructure (Proxmox VE), Systems Administration (Windows/Linux) and Network Segmentation**

> **Status: M0 completed — Kickoff and planning.** This README documents the scope, target architecture and traceability model. Implementation will be recorded incrementally in the commit history and in `docs/`. Validation evidence is stored in `docs/validation/`.

## 1. Purpose and professional context

Mittelstand-IT-Lab is a personal infrastructure laboratory designed to demonstrate practical skills in Windows/Linux systems administration, virtualization, network segmentation and IT infrastructure security, with a focus on the real needs of the German mid-sized enterprise (Mittelstand).

The lab stems from **12 years operating critical infrastructures** (international airport, banking). It therefore prioritizes **verified recovery**, **least-privilege segmentation** and **auditable traceability**, not just a functional test environment.

The project is managed as a formal lifecycle: planning → design → implementation → validation → documentation. The initial commit constitutes the project charter.

## 2. Scope and objectives

**In scope**
- Ubuntu **26.04.1 LTS** host with KVM/QEMU and libvirt as the virtualization layer.
- **Proxmox VE 9.2-1** deployed as a VM on that layer.
- **OPNsense 26.7** as perimeter firewall and segmentation engine.
- Workloads: **Debian 13** and **Windows Server 2025**.
- Network segmentation via VLANs and planned IPv4 addressing.
- Monitoring and backup strategy with recovery criteria.

**Out of scope (v1)**
- Internet exposure of production services.
- Multi-node high availability (to be evaluated in v2).
- Integration with cloud providers.

## 3. Target architecture

| Layer | Component | Function |
|---|---|---|
| Host | Ubuntu 26.04.1 LTS | Host system, virtualization base |
| Virtualization | KVM/QEMU + libvirt | Virtualization engine on host |
| Platform | Proxmox VE 9.2-1 (VM) | VM, storage and network management |
| Edge | OPNsense 26.7 | Firewall, NAT, VLAN segmentation |
| Workloads | Debian 13.7.0 / Windows Server 2025 | Services and administration tests |
| Operations | Monitoring + Backup | Observability and recovery |

The topology diagram will be published in `docs/architecture/` at milestone M2.

## 4. Roadmap and acceptance criteria

| Milestone | Deliverable | Acceptance criterion | Status |
|---|---|---|---|
| M0 | README + documented scope | Initial commit with scope, architecture and traceability model | ✅ Completed |
| M1 | KVM/QEMU host operational | Nesting validated: `/dev/kvm` present inside the L1 VM, `host-passthrough` CPU model, `virsh list` shows the VM in `running`, evidence in `docs/validation/` | 🟨 In progress |
| M2 | Proxmox VE deployed | Proxmox accessible via web, topology diagram v1 published, L1 VM configuration documented | ⬜ Pending |
| M3 | OPNsense + segmentation | VLANs defined, flow matrix documented, isolation and permission tests executed with evidence | ⬜ Pending |
| M4 | Linux/Windows workloads | At least one Debian server and one Windows server integrated, network routes validated | ⬜ Pending |
| M5 | Monitoring and backup | Metrics visible, backup restore verified and documented as evidence | ⬜ Pending |

Each milestone is closed with: entry in `docs/`, update of this table, validation evidence in `docs/validation/` and a descriptive commit.

## 5. Traceability model

- **Commit convention:** `type(scope): description` — e.g. `docs(architecture): add network topology v1`.
- **Documentation per phase:** each milestone generates a document in `docs/NN-phase.md`.
- **Decision log:** relevant technical decisions are recorded as ADRs in `docs/adr/`.
- **Requirement → implementation → validation traceability:** each scope requirement references its milestone and validation evidence in `docs/validation/`.
- **History as evidence:** the commit history is the source of truth for project progress.
- **Process ADR:** process decisions (e.g. translation strategy, see
  `docs/adr/001-ai-translation.en.md`) are also recorded as ADRs in
  `docs/adr/`, with the same format as technical ADRs.

## 6. Risk management (extract)

| Risk | Impact | Mitigation |
|---|---|---|
| Nested virtualization complexity | Medium | Validate KVM and `/dev/kvm` inside the L1 VM before deploying Proxmox (M1) |
| Segmentation errors | High | Document flow matrix, test per-VLAN connectivity, default-deny |
| Configuration loss / drift | High | Versioned backup, snapshots, verified restore (M5) |
| Single point of failure (one host) | Medium | Conscious acceptance in v1, HA evaluation in v2 |
| Scope drift | Medium | Roadmap with closed acceptance criteria |

## 7. Documentation and evidence

Project documentation is maintained in `docs/`:
- `docs/00-scope.md` — scope and objectives
- `docs/architecture/` — diagrams, IP plan and design decisions
- `docs/adr/` — architecture and process decision records (includes `001-ai-translation.en.md`, translation strategy)
- `docs/NN-phase.md` — documentation per milestone
- `docs/validation/` — validation evidence (screenshots, command outputs, test results)

## 8. Notes on language and authorship

- **Source of authorship:** Spanish (`README.es.md`). The technical content
  (architecture, acceptance criteria, validation evidence) is original work.
- **Authoritative publication version:** German (`README.md`), oriented
  to the target market (German Mittelstand).
- **Secondary version:** English (`README.en.md`).
- **Translation:** DE and EN are generated with AI assistance and reviewed
  manually. AI is used exclusively for the linguistic version,
  not for technical content.
- **Synchronization:** on content changes, the Spanish source is updated first
  and then DE/EN are synchronized (ES → DE/EN).
- Decision recorded in [`docs/adr/001-ai-translation.en.md`](docs/adr/001-ai-translation.en.md).

## 9. Project status

**Current phase: M0 completed — Kickoff and planning. M1 in progress.**
The infrastructure is not yet fully deployed. Architecture design, network planning, implementation and testing will be documented as the project progresses, maintaining full traceability from this initial commit. The first validation evidence will be published in `docs/validation/` when M1 is closed.