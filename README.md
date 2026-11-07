# Mittelstand-IT-Lab

[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

**Verschachtelte virtuelle Infrastruktur (Proxmox VE), Systemadministration (Windows/Linux) und Netzwerksegmentierung**

> **Status: M0 abgeschlossen — Kickoff und Planung.** Diese README dokumentiert den Umfang, die Zielarchitektur und das Traceability-Modell. Die Implementierung wird schrittweise in der Commit-Historie und in `docs/` dokumentiert. Validierungsnachweise werden in `docs/validation/` gespeichert.

## 1. Zweck und beruflicher Kontext

Mittelstand-IT-Lab ist ein persönliches Infrastrukturlabor, das praktische Kompetenzen in der Windows/Linux-Systemadministration, Virtualisierung, Netzwerksegmentierung und IT-Infrastruktursicherheit demonstrieren soll, mit Fokus auf die realen Anforderungen des deutschen Mittelstands.

Das Labor entstand aus **12 Jahren Betrieb kritischer Infrastrukturen** (internationaler Flughafen, Bankwesen). Daher priorisiert es die **verifizierte Wiederherstellung**, die **Segmentierung nach dem Least-Privilege-Prinzip** und die **auditierbare Rückverfolgbarkeit**, nicht nur eine funktionale Testumgebung.

Das Projekt wird als formaler Lebenszyklus geführt: Planung → Design → Implementierung → Validierung → Dokumentation. Der initiale Commit stellt die Projekturkunde dar.

## 2. Umfang und Ziele

**Im Umfang enthalten**
- Ubuntu **26.04.1 LTS** Host mit KVM/QEMU und libvirt als Virtualisierungsschicht.
- **Proxmox VE 9.2-1** als VM auf dieser Schicht bereitgestellt.
- **OPNsense 26.7** als Perimeter-Firewall und Segmentierungs-Engine.
- Workloads: **Debian 13** und **Windows Server 2025**.
- Netzwerksegmentierung mittels VLANs und geplanter IPv4-Adressierung.
- Monitoring und Backup-Strategie mit Wiederherstellungskriterien.

**Nicht im Umfang (v1)**
- Internet-Exposition von Produktionsdiensten.
- Hochverfügbarkeit über mehrere Knoten (Bewertung in v2).
- Integration mit Cloud-Anbietern.

## 3. Zielarchitektur

| Schicht | Komponente | Funktion |
|---|---|---|
| Host | Ubuntu 26.04.1 LTS | Host-System, Virtualisierungsbasis |
| Virtualisierung | KVM/QEMU + libvirt | Virtualisierungs-Engine auf dem Host |
| Plattform | Proxmox VE 9.2-1 (VM) | VM-, Speicher- und Netzwerkverwaltung |
| Edge | OPNsense 26.7 | Firewall, NAT, VLAN-Segmentierung |
| Workloads | Debian 13.7.0 / Windows Server 2025 | Dienste und Administrationstests |
| Betrieb | Monitoring + Backup | Observability und Wiederherstellung |

Das Topologie-Diagramm wird in `docs/architecture/` zum Meilenstein M2 veröffentlicht.

## 4. Roadmap und Abnahmekriterien

| Meilenstein | Deliverable | Abnahmekriterium | Status |
|---|---|---|---|
| M0 | README + dokumentierter Umfang | Initialer Commit mit Umfang, Architektur und Traceability-Modell | ✅ Abgeschlossen |
| M1 | KVM/QEMU-Host betriebsbereit | Nesting validiert: `/dev/kvm` in der L1-VM vorhanden, CPU-Modell `host-passthrough`, `virsh list` zeigt die VM im Status `running`, Nachweis in `docs/validation/` | 🟨 In Bearbeitung |
| M2 | Proxmox VE bereitgestellt | Proxmox über Web erreichbar, Topologie-Diagramm v1 veröffentlicht, L1-VM-Konfiguration dokumentiert | ⬜ Ausstehend |
| M3 | OPNsense + Segmentierung | VLANs definiert, Flow-Matrix dokumentiert, Isolations- und Berechtigungstests mit Nachweis durchgeführt | ⬜ Ausstehend |
| M4 | Linux/Windows-Workloads | Mindestens ein Debian-Server und ein Windows-Server integriert, Netzwerkrouten validiert | ⬜ Ausstehend |
| M5 | Monitoring und Backup | Metriken sichtbar, Backup-Wiederherstellung verifiziert und als Nachweis dokumentiert | ⬜ Ausstehend |

Jeder Meilenstein wird abgeschlossen mit: Eintrag in `docs/`, Aktualisierung dieser Tabelle, Validierungsnachweis in `docs/validation/` und einem beschreibenden Commit.

## 5. Traceability-Modell

- **Commit-Konvention:** `typ(scope): beschreibung` — z. B. `docs(architecture): add network topology v1`.
- **Dokumentation pro Phase:** jeder Meilenstein erzeugt ein Dokument in `docs/NN-phase.md`.
- **Entscheidungsprotokoll:** relevante technische Entscheidungen werden als ADR in `docs/adr/` festgehalten.
- **Traceability Anforderung → Implementierung → Validierung:** jede Anforderung aus dem Umfang referenziert ihren Meilenstein und ihren Validierungsnachweis in `docs/validation/`.
- **Historie als Nachweis:** die Commit-Historie ist die Quelle der Wahrheit für den Projektfortschritt.
- **Prozess-ADR:** Prozessentscheidungen (z. B. Übersetzungsstrategie, siehe
  `docs/adr/001-ai-translation.md`) werden ebenfalls als ADRs in
  `docs/adr/` festgehalten, im selben Format wie technische ADRs.

## 6. Risikomanagement (Auszug)

| Risiko | Auswirkung | Gegenmaßnahme |
|---|---|---|
| Komplexität der verschachtelten Virtualisierung | Mittel | KVM und `/dev/kvm` in der L1-VM vor Proxmox-Bereitstellung validieren (M1) |
| Segmentierungsfehler | Hoch | Flow-Matrix dokumentieren, Konnektivität pro VLAN testen, default-deny |
| Konfigurationsverlust / Drift | Hoch | Versionierte Backups, Snapshots, verifizierte Wiederherstellung (M5) |
| Single Point of Failure (ein Host) | Mittel | Bewusste Akzeptanz in v1, HA-Bewertung in v2 |
| Scope-Drift | Mittel | Roadmap mit geschlossenen Abnahmekriterien |

## 7. Dokumentation und Nachweise

Die Projektdokumentation wird in `docs/` geführt:
- `docs/00-scope.md` — Umfang und Ziele
- `docs/architecture/` — Diagramme, IP-Plan und Designentscheidungen
- `docs/adr/` — Architektur- und Prozessentscheidungsprotokolle (inkl. `001-ai-translation.md`, Übersetzungsstrategie)
- `docs/NN-phase.md` — Dokumentation pro Meilenstein
- `docs/validation/` — Validierungsnachweise (Screenshots, Kommandoausgaben, Testergebnisse)

## 8. Hinweise zu Sprache und Autorschaft

- **Autorschaftsquelle:** Spanisch (`README.es.md`). Der technische Inhalt
  (Architektur, Abnahmekriterien, Validierungsnachweise) ist Eigenarbeit.
- **Autoritative Veröffentlichungsversion:** Deutsch (`README.md`), ausgerichtet
  auf den Zielmarkt (deutscher Mittelstand).
- **Sekundärversion:** Englisch (`README.en.md`).
- **Übersetzung:** DE und EN werden mit KI-Unterstützung erstellt und
  manuell überprüft. KI wird ausschließlich für die sprachliche Version
  eingesetzt, nicht für den technischen Inhalt.
- **Synchronisierung:** bei Inhaltsänderungen wird zuerst die spanische Quelle
  aktualisiert und anschließend DE/EN synchronisiert (ES → DE/EN).
- Entscheidung dokumentiert in [`docs/adr/001-ai-translation.md`](docs/adr/001-ai-translation.md).

## 9. Projektstatus

**Aktuelle Phase: M0 abgeschlossen — Kickoff und Planung. M1 in Bearbeitung.**
Die Infrastruktur ist noch nicht vollständig bereitgestellt. Architekturdesign, Netzwerkplanung, Implementierung und Tests werden im Projektverlauf dokumentiert, wobei die vollständige Rückverfolgbarkeit ab diesem initialen Commit gewahrt bleibt. Die ersten Validierungsnachweise werden in `docs/validation/` veröffentlicht, sobald M1 abgeschlossen ist.