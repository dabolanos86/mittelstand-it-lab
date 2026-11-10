[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

# Logische und Netzwerk-Architektur — Mittelstand-IT-Lab

**Dokument:** Architektur-Index  
**Version:** 1.0  
**Meilenstein:** M0 - Kickoff & Planung  
**Status:** Vorgeschlagen  
**Datum:** 2026-11-10  

## 📌 Zweck dieses Verzeichnisses

Dieses Verzeichnis enthält die logischen Design-Artefakte der Laborinfrastruktur. Hier wird definiert, **wie** das System aufgebaut ist (Virtualisierungsschichten), **wie** die Komponenten kommunizieren (Netzwerktopologie) und **welche** IP-Adressen und VLANs verwendet werden.

Die gesamte Dokumentation hier repräsentiert den Zustand **"As-Designed"** (Entworfen). Die Nachweise, dass das System wie entworfen funktioniert, finden sich in `docs/validation/`, sobald die Implementierungsmeilensteine (M1 und folgende) ausgeführt wurden.

## 🗂️ Artefakt-Index

Die in diesem Ordner verfügbaren Dokumente und Diagramme sind im Folgenden aufgeführt:

| Artefakt | Typ | Beschreibung |
| :--- | :--- | :--- |
| [`architecture-v1.png`](./architecture-v1.png) | Diagramm | Hierarchische Ansicht der Infrastruktur (L0 Physischer Host, L1 Verschachteltes Proxmox, L2 VMs). |
| [`architecture-v1.drawio`](./architecture-v1.drawio) | Quelle | Bearbeitbare Datei des hierarchischen Diagramms. |
| [`vlan-plan-v1.png`](./vlan-plan-v1.png) | Diagramm | Visuelle Tabelle des VLAN-Plans, der Subnetze (VLSM) und Gateways. |
| [`vlan-plan-v1.drawio`](./vlan-plan-v1.drawio) | Quelle | Bearbeitbare Datei des VLAN-Plans. |
| [`ip-plan.md`](./ip-plan.md) | Dokument | Detaillierter IP-Adressierungsplan, VM-Inventar und Firewall-Schnittstellen. |
| [`flow-matrix.md`](./flow-matrix.md) | Dokument | Firewall-Flow-Matrix (Default-Deny und Ausnahmen). |

## 🔗 Beziehung zwischen den Artefakten

*   Das **Architekturdiagramm** (`architecture-v1.png`) zeigt die physische und virtuelle Hierarchie.
*   Der **VLAN-Plan** (`vlan-plan-v1.png`) definiert die L2-Segmentierung.
*   Der **IP-Plan** (`ip-plan.md`) übersetzt diese Segmentierung in konkrete Adressen für jede VM.
*   Die **Flow-Matrix** (`flow-matrix.md`) legt fest, welcher Datenverkehr zwischen diesen IPs durch die Firewall fließen darf.

## 🛠️ Bearbeitungswerkzeuge

Dateien mit der Erweiterung `.drawio` wurden mit [Draw.io] erstellt, die `.md`-Dateien wurden mit [Ghostwriter] erstellt, beide auf einem Ubuntu-Betriebssystem. Um sie zu ändern, wird empfohlen, sie mit diesen Werkzeugen (Web oder Desktop) zu öffnen und bei `.drawio`-Dateien das Ergebnis im hochauflösenden `.png`-Format zu exportieren, um die visuelle Kohärenz zu wahren.