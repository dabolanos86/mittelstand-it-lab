[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

# Architecture Decision Records (ADRs)

**Dokument:** Architektur-Entscheidungsindex  
**Version:** 1.0  
**Meilenstein:** M0 - Kickoff & Planung  
**Status:** Vorgeschlagen  
**Datum:** 2026-11-10

## 📌 Was ist ein ADR?

Ein **Architecture Decision Record (ADR)** ist ein kurzes Dokument, das eine wichtige Architektur- und Prozessentscheidung zusammen mit ihrem Kontext und ihren Konsequenzen festhält. Sein Zweck ist es, **Rückverfolgbarkeit** darüber zu bieten, *warum* eine Entscheidung getroffen wurde, nicht nur *was* entschieden wurde.

In diesem Projekt folgen ADRs dem Standardformat von Michael Nygard:
- **Kontext:** Die Situation, die die Entscheidung motiviert.
- **Entscheidung:** Die gewählte Option.
- **Alternativen:** Die betrachteten und verworfenen Optionen.
- **Konsequenzen:** Die positiven und negativen Auswirkungen der Entscheidung.

## 📋 Konventionen

- **Sequenzielle Nummerierung:** `NNN-beschreibender-titel.sprache.md` (z. B. `001-ai-translation.es.md`).
- **Unveränderlichkeit:** Ein akzeptierter ADR wird nicht geändert. Wenn sich eine Entscheidung ändert, wird ein neuer ADR erstellt, der den vorherigen ersetzt (z. B. `005-replace-adr-002.md`).
- **Sprachen:** Jeder ADR wird auf Spanisch (Quelle), Englisch und Deutsch veröffentlicht, mit Querverweisen in der Kopfzeile.
- **Status:** `Vorgeschlagen`, `Akzeptiert`, `Abgelehnt`, `Ersetzt durch NNN`.

## 🗂️ ADR-Index

| ID | Titel | Meilenstein | Status | Sprachen |
| :--- | :--- | :--- | :--- | :--- |
| [001](./001-ai-translation.es.md) | Übersetzungsstrategie und Sprachversionen der README | M0 | ✅ Akzeptiert | [ES](./001-ai-translation.es.md) · [EN](./001-ai-translation.en.md) · [DE](./001-ai-translation.md) |
| [002](./002-nested-virtualization.es.md) | Verschachtelte Virtualisierung: Proxmox VE auf KVM/QEMU | M0 | ⚠️ Ersetzt durch [ADR-005](./005-replace-adr-002.md) | [ES](./002-nested-virtualization.es.md) · [EN](./002-nested-virtualization.en.md) · [DE](./002-nested-virtualization.de.md) |
| [003](./003-opnsense-vs-pfsense.md) | Wahl von OPNsense gegenüber pfSense als Perimeter-Firewall | M0 | ✅ Akzeptiert | [ES](./003-opnsense-vs-pfsense.es.md) · [EN](./003-opnsense-vs-pfsense.en.md) · [DE](./003-opnsense-vs-pfsense.md) |
| [004](./004-vlan-design.md) | VLAN-Design und VLSM-Adressierung | M0 | ✅ Akzeptiert | [ES](./004-vlan-design.es.md) · [EN](./004-vlan-design.en.md) · [DE](./004-vlan-design.md) |
| [005](./005-replace-adr-002.md) | Ersetzung des ADR-002 aufgrund einer Softwareversionskorrektur | M0 | ✅ Akzeptiert | [ES](./005-replace-adr-002.es.md) · [EN](./005-replace-adr-002.en.md) · [DE](./005-replace-adr-002.md) |

> **Hinweis:** Die Tabelle wird aktualisiert, sobald in späteren Meilensteinen (M1, M2, M3...) neue ADRs hinzugefügt werden.

## 🔗 Beziehung zu anderen Dokumenten

- **`docs/architecture/`**: Enthält die Diagramme und Pläne, die die hier festgehaltenen Entscheidungen umsetzen.
- **`docs/validation/`**: Enthält die Nachweise, die belegen, dass die Entscheidungen in der Praxis funktionieren.
- **`README.md` (Stammverzeichnis)**: Abschnitt 5 („Traceability-Modell") erklärt, wie ADRs in den Projektlebenszyklus passen.