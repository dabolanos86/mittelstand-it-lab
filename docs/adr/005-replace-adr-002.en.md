[Deutsch](005-replace-adr-002.md) | [English](005-replace-adr-002.en.md) | [Español](005-replace-adr-002.es.md)

# ADR 005 — Replacement of ADR-002 due to software version correction

- **Status:** Accepted
- **Milestone:** M0
- **Date:** 2026-11-09

## Context

ADR-002 (Nested Virtualization) specified software versions that do not match the project's reality. Specifically, it indicated `Ubuntu 24.04` (a version that changed) and `Proxmox VE 8.2` (a version that changed). These inconsistencies affect the coherence of the design and the traceability of the project. A documentation error was made, as the project was conceived with later versions.

According to the immutability convention of ADRs defined in `docs/adr/README.en.md`, an accepted ADR is not modified. Therefore, the correction must be documented by means of a new ADR that replaces the previous one.

## Decision

The software versions in ADR-002 are corrected, establishing:
- **Proxmox VE 9.2-1** (current stable version).
- **Ubuntu 26.04.1 LTS** (current stable version).

**ADR-002 is replaced by this ADR-005**. It is kept in the repository for historical traceability.

## Alternatives considered

- **Editing ADR-002 directly:** Violates the immutability rule of accepted ADRs.
- **Leaving ADR-002 with the erroneous versions:** Breaks the project's coherence and confuses readers.
- **Creating a new ADR (this option):** Respects immutability and maintains traceability.

## Consequences

**Positive:**
- Coherence in software versions across all documents.
- Historical traceability: it can be seen what was decided in ADR-002 and why it was changed in ADR-005.
- Respect for the ADR immutability rule.

**Negative:**
- Greater number of documents in the repository.
- Requires updating the ADR index and adding a replacement note in ADR-002.

## References

- [`docs/adr/002-nested-virtualization.en.md`](./002-nested-virtualization.en.md) — Replaced ADR.
- [`docs/adr/README.en.md`](./README.en.md) — Updated ADR index.