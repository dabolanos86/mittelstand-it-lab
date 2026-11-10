# Mittelstand-IT-Lab

[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

**Verschachtelte virtuelle Infrastruktur (Proxmox VE), Systemadministration (Windows/Linux) und Netzwerksegmentierung**

> **Status: M0 abgeschlossen — Kickoff und Planung.** Diese README ist die Startseite des Projekts. Die vollständigen Details zu Umfang, Zielen, Erfolgskriterien und Risiken finden sich in [`docs/00-scope.md`](docs/00-scope.md). Die Implementierung wird schrittweise in der Commit-Historie und in `docs/` dokumentiert.

## 1. Zweck und beruflicher Kontext

Mittelstand-IT-Lab ist ein persönliches Infrastrukturlabor, das praktische Kompetenzen in der Windows/Linux-Systemadministration, verschachtelten Virtualisierung, Netzwerksegmentierung und IT-Infrastruktursicherheit demonstrieren soll, mit Fokus auf die realen Anforderungen des deutschen Mittelstands.

Das Labor entstand aus **12 Jahren Betrieb kritischer Infrastrukturen** (internationaler Flughafen, Bankwesen). Daher priorisiert es die **verifizierte Wiederherstellung**, die **Segmentierung nach dem Least-Privilege-Prinzip** und die **auditierbare Rückverfolgbarkeit**, nicht nur eine funktionale Testumgebung.

Der vollständige Umfang, die technischen und Lernziele, die Erfolgskriterien, Einschränkungen, Annahmen und Risiken sind in [`docs/00-scope.md`](docs/00-scope.md) dokumentiert.

## 2. Zielarchitektur

| Schicht | Komponente | Funktion |
|---|---|---|
| Host (L0) | Ubuntu 26.04.1 LTS | Host-System, Virtualisierungsbasis |
| Virtualisierung | KVM/QEMU + libvirt | Virtualisierungs-Engine auf dem Host |
| Plattform (L1) | Proxmox VE 9.2-1 (VM) | VM-, Speicher- und Netzwerkverwaltung |
| Edge (L2) | OPNsense 26.7 | Firewall, NAT, VLAN-Segmentierung |
| Workloads | Debian 13 / Windows Server 2025 | Dienste und Administrationstests |
| Betrieb | Monitoring + Backup | Observability und Wiederherstellung |

Das Architekturdiagramm befindet sich in [`docs/architecture/architecture-v1.png`](docs/architecture/architecture-v1.png). Der IP-Plan und die Flow-Matrix befinden sich in [`docs/architecture/`](docs/architecture/).

## 3. Roadmap-Zusammenfassung

| Meilenstein | Deliverable | Status |
|---|---|---|
| M0 | README, Umfang, Architektur und ADRs | ✅ Abgeschlossen |
| M1 | KVM/QEMU-Host betriebsbereit und validiert | ⬜ Ausstehend |
| M2 | Proxmox VE bereitgestellt und zugänglich | ⬜ Ausstehend |
| M3 | OPNsense + VLAN-Segmentierung | ⬜ Ausstehend |
| M4 | Linux/Windows-Workloads integriert | ⬜ Ausstehend |
| M5 | Monitoring und verifiziertes Backup | ⬜ Ausstehend |

Die detaillierten Abnahmekriterien für jeden Meilenstein finden sich in [`docs/00-scope.md`](docs/00-scope.md) (Abschnitt 4).

## 4. Traceability-Modell

- **Commit-Konvention:** `typ(scope): beschreibung` — z. B. `docs(architecture): add network topology v1`.
- **Dokumentation pro Phase:** jeder Meilenstein erzeugt ein Dokument in `docs/NN-phase.md`.
- **Entscheidungsprotokoll:** relevante technische Entscheidungen werden als ADRs in [`docs/adr/`](docs/adr/) festgehalten.
- **Traceability Anforderung → Implementierung → Validierung:** jede Anforderung aus dem Umfang referenziert ihren Meilenstein und ihren Validierungsnachweis in [`docs/validation/`](docs/validation/).
- **Historie als Nachweis:** die Commit-Historie ist die Quelle der Wahrheit für den Projektfortschritt.

## 5. Dokumentation und Nachweise

Die Projektdokumentation wird in `docs/` geführt:
- [`docs/00-scope.md`](docs/00-scope.md) — Umfang, Ziele, Erfolgskriterien, Risiken.
- [`docs/architecture/`](docs/architecture/) — Diagramme, IP-Plan und Flow-Matrix.
- [`docs/adr/`](docs/adr/) — Architektur- und Prozessentscheidungsprotokolle.
- [`docs/validation/`](docs/validation/) — Validierungsnachweise (Screenshots, Kommandoausgaben, Testergebnisse).

## 6. Hinweise zu Sprache und Autorschaft

- **Autorschaftsquelle:** Spanisch (`README.es.md`). Der technische Inhalt (Architektur, Abnahmekriterien, Validierungsnachweise) ist Eigenarbeit.
- **Autoritative Veröffentlichungsversion:** Deutsch (`README.md`), ausgerichtet auf den Zielmarkt (deutscher Mittelstand).
- **Sekundärversion:** Englisch (`README.en.md`).
- **Übersetzung:** DE und EN werden mit KI-Unterstützung erstellt und manuell überprüft. KI wird ausschließlich für die sprachliche Version eingesetzt, nicht für den technischen Inhalt.
- **Synchronisierung:** bei Inhaltsänderungen wird zuerst die spanische Quelle aktualisiert und anschließend DE/EN synchronisiert (ES → DE/EN).
- Entscheidung dokumentiert in [`docs/adr/001-ai-translation.md`](docs/adr/001-ai-translation.md).

## 7. Projektstatus

**Aktuelle Phase: M0 abgeschlossen — Kickoff und Planung. M1 in Bearbeitung.**
Die Infrastruktur ist noch nicht vollständig bereitgestellt. Architekturdesign, Netzwerkplanung, Implementierung und Tests werden im Projektverlauf dokumentiert, wobei die vollständige Rückverfolgbarkeit ab diesem initialen Commit gewahrt bleibt. Die ersten Validierungsnachweise werden in [`docs/validation/`](docs/validation/) veröffentlicht, sobald M1 abgeschlossen ist.