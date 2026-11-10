[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

# Projektdokumentation — Mittelstand-IT-Lab

**Dokument:** Allgemeiner Dokumentationsindex  
**Version:** 1.0  
**Meilenstein:** M0 - Kickoff & Planung  
**Status:** Vorgeschlagen  
**Datum:** 2026-11-10  

## 📌 Zweck dieses Verzeichnisses

Dieser Ordner enthält die technische Dokumentation des Projekts, organisiert nach Meilensteinen und Artefakttypen. Hier wird der vollständige Lebenszyklus festgehalten: Planung (M0), Implementierung (M1-M2), Segmentierung (M3), Workloads (M4) und Betrieb (M5).

## 🗂️ Allgemeiner Index

### Übergreifende Dokumente

| Dokument | Beschreibung | Sprachen |
| :--- | :--- | :--- |
| [`00-scope.md`](./00-scope.md) | Projektumfang, Ziele, Erfolgskriterien und Risiken. | [ES](./00-scope.es.md) · [EN](./00-scope.en.md) · [DE](./00-scope.md) |

### Architektur und Netzwerk

| Dokument | Beschreibung | Sprachen |
| :--- | :--- | :--- |
| [`architecture/`](./architecture/) | Diagramme, IP-Plan und Flow-Matrix. | [ES](./architecture/README.es.md) · [EN](./architecture/README.en.md) · [DE](./architecture/README.md) |

### Architecture Decision Records (ADRs)

| Dokument | Beschreibung | Sprachen |
| :--- | :--- | :--- |
| [`adr/`](./adr/) | Protokoll technischer und prozessualer Entscheidungen. | [ES](./adr/README.es.md) · [EN](./adr/README.en.md) · [DE](./adr/README.md) |

### Dokumentation pro Meilenstein

| Meilenstein | Dokument | Status |
| :--- | :--- | :--- |
| M0 | `00-scope.es.md` | ✅ Abgeschlossen |
| M1 | `01-host-kvm-audit.md` | ⬜ Ausstehend |
| M2 | `02-proxmox-deployment.md` | ⬜ Ausstehend |
| M3 | `03-opnsense-segmentation.md` | ⬜ Ausstehend |
| M4 | `04-workloads-deployment.md` | ⬜ Ausstehend |
| M5 | `05-monitoring-backup.md` | ⬜ Ausstehend |

### Validierungsnachweise

| Ordner | Beschreibung |
| :--- | :--- |
| [`validation/`](./validation/) | Screenshots, Kommandoausgaben und Testergebnisse. |

## 🔗 Beziehung zu anderen Dokumenten

- **`README.md` (Stammverzeichnis)**: Hauptprojektdokument (Projekturkunde).
- **`docs/architecture/`**: Logisches und Netzwerk-Design.
- **`docs/adr/`**: Architektur- und Prozessentscheidungen.
- **`docs/validation/`**: Nachweise, dass das System wie entworfen funktioniert.

## 🛠️ Konventionen

- **Nummerierung pro Meilenstein:** `NN-beschreibender-titel.md` (z. B. `01-host-kvm-audit.md`).
- **Sprachen:** Jedes Dokument wird auf Spanisch (Quelle), Englisch und Deutsch veröffentlicht.
- **Status:** `Vorgeschlagen`, `Akzeptiert`, `In Bearbeitung`, `Abgeschlossen`.