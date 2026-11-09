[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

# Architecture Decision Records (ADRs)

**Document:** Architecture Decision Index  
**Version:** 1.0  
**Milestone:** M0 - Kickoff & Planning  
**Status:** Proposed  
**Date:** 2026-11-09

## 📌 What is an ADR?

An **Architecture Decision Record (ADR)** is a short document that captures an important architecture and process decision, along with its context and consequences. Its purpose is to provide **traceability** on *why* a decision was made, not just *what* was decided.

In this project, ADRs follow Michael Nygard's standard format:
- **Context:** The situation that motivates the decision.
- **Decision:** The chosen option.
- **Alternatives:** The options considered and discarded.
- **Consequences:** The positive and negative effects of the decision.

## 📋 Conventions

- **Sequential numbering:** `NNN-descriptive-title.language.md` (e.g. `001-ai-translation.es.md`).
- **Immutability:** An accepted ADR is not modified. If a decision changes, a new ADR is created that replaces the previous one (e.g. `005-replace-adr-002.md`).
- **Languages:** Each ADR is published in Spanish (source), English and German, with cross-links in the header.
- **Status:** `Proposed`, `Accepted`, `Rejected`, `Superseded by NNN`.

## 🗂️ ADR Index

| ID | Title | Milestone | Status | Languages |
| :--- | :--- | :--- | :--- | :--- |
| [001](./001-ai-translation.en.md) | Translation strategy and README language versions | M0 | ✅ Accepted | [ES](./001-ai-translation.es.md) · [EN](./001-ai-translation.en.md) · [DE](./001-ai-translation.md) |
| [002](./002-nested-virtualization.en.md) | Nested virtualization: Proxmox VE on KVM/QEMU | M0 | ✅ Accepted | [ES](./002-nested-virtualization.es.md) · [EN](./002-nested-virtualization.en.md) · [DE](./002-nested-virtualization.md) |
| [003](./003-opnsense-vs-pfsense.en.md) | Choosing OPNsense over pfSense as Perimeter Firewall | M0 | ✅ Accepted | [ES](./003-opnsense-vs-pfsense.es.md) · [EN](./003-opnsense-vs-pfsense.en.md) · [DE](./003-opnsense-vs-pfsense.md) |

> **Note:** The table will be updated as new ADRs are added in subsequent milestones (M1, M2, M3...).

## 🔗 Relationship with other documents

- **`docs/architecture/`**: Contains the diagrams and plans that implement the decisions recorded here.
- **`docs/validation/`**: Contains the evidence that demonstrates the decisions work in practice.
- **`README.md` (root)**: Section 5 ("Traceability model") explains how ADRs fit into the project lifecycle.