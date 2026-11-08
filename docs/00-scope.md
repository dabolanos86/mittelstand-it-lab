[Deutsch](00-scope.md) | [English](00-scope.en.md) | [Español](00-scope.es.md)

# Umfangs- und Ziel Dokument — Mittelstand-IT-Lab

**Dokument:** 00 - Scope & Objectives  
**Version:** 1.0  
**Meilenstein:** M0 - Kickoff & Planung  
**Status:** Vorgeschlagen  
**Autor:** Ing. Daniel Alejandro Bolanos Solana  
**Datum:** 2026-11-08  

## 1. Projektzweck

Mittelstand-IT-Lab ist ein persönliches Infrastrukturlabor, das praktische Kompetenzen in der Windows/Linux-Systemadministration, verschachtelten Virtualisierung, Netzwerksegmentierung und IT-Infrastruktursicherheit demonstrieren soll, mit Fokus auf die realen Anforderungen des deutschen Mittelstands.

Das Labor entstand aus **12 Jahren Betrieb kritischer Infrastrukturen** (internationaler Flughafen, Bankwesen). Daher priorisiert es die **verifizierte Wiederherstellung**, die **Segmentierung nach dem Least-Privilege-Prinzip** und die **auditierbare Rückverfolgbarkeit**, nicht nur eine funktionale Testumgebung.

## 2. Projektumfang

### 2.1 Im Umfang enthalten (v1)

| Komponente | Beschreibung |
| :--- | :--- |
| **Physischer Host (L0)** | Ubuntu 24.04 LTS mit KVM/QEMU und libvirt als Virtualisierungsschicht. |
| **Plattform (L1)** | Proxmox VE 8.2 als VM auf der L0-Schicht bereitgestellt. |
| **Edge (L2)** | OPNsense 24.1 als Perimeter-Firewall und Segmentierungs-Engine. |
| **Windows-Workloads** | Windows Server 2025 (DC01, DC02) mit AD-, DHCP- und DNS-Rollen. |
| **Linux-Workloads** | Debian 13 (MON01 für Zabbix, Client02 für kritische Tests). |
| **Clients** | Windows 11 (Client01) und Debian 13 (Client02). |
| **Netzwerk** | Segmentierung mittels VLANs und geplanter IPv4-Adressierung (VLSM). |
| **Betrieb** | Monitoring (Zabbix) und Backup-Strategie mit Wiederherstellungskriterien. |

### 2.2 Nicht im Umfang (v1)

| Element | Ausschlussgrund |
| :--- | :--- |
| Internet-Exposition von Produktionsdiensten | Das Labor ist für den persönlichen Gebrauch; es werden keine Dienste ins Internet exponiert. |
| Hochverfügbarkeit über mehrere Knoten (HA) | Die Hardware ist ein einzelner Laptop; Bewertung in v2. |
| Integration mit Cloud-Anbietern | Nicht notwendig für die Lernziele. |
| Einzelhandelslizenzen für Windows Server | Es wird die offizielle Evaluierungsversion verwendet (180 Tage, erneuerbar). |
| 24/7-Support | Das Labor ist keine Produktionsumgebung. |

## 3. Projektziele

### 3.1 Technische Ziele

1.  Validierung der verschachtelten Virtualisierung (KVM in KVM) auf einem Ubuntu-Host.
2.  Bereitstellung von Proxmox VE als Level-1-Hypervisor auf der L0-Schicht.
3.  Implementierung einer Perimeter-Firewall (OPNsense) mit VLAN-Segmentierung.
4.  Bereitstellung einer Windows Server-Domäne (Active Directory, DNS, DHCP).
5.  Integration von Linux-Servern (Debian) und Windows/Linux-Clients in die Domäne.
6.  Implementierung von Monitoring (Zabbix) und einer verifizierbaren Backup-Strategie.

### 3.2 Lernziele

1.  Dokumentation jeder technischen Entscheidung durch ADRs (Architecture Decision Records).
2.  Führung einer atomaren und beschreibenden Commit-Historie.
3.  Erstellung von Validierungsnachweisen in `docs/validation/` für jeden Meilenstein.
4.  Anwendung des **Default-Deny**-Prinzips in der Firewall-Richtlinie.
5.  Praxis der Windows- und Linux-Systemadministration in einer segmentierten Umgebung.

## 4. Erfolgskriterien

Das Projekt gilt als erfolgreich, wenn am Ende von M5 die folgenden Kriterien erfüllt sind:

| Kriterium | Nachweis |
| :--- | :--- |
| Der L0-Host hat KVM/QEMU betriebsbereit und `/dev/kvm` zugänglich. | Screenshot von `egrep -c '(vmx|svm)' /proc/cpuinfo` und `virsh list`. |
| Proxmox VE ist über Web erreichbar und verwaltet die VMs. | Screenshot des Proxmox-Panels. |
| OPNsense segmentiert den Datenverkehr gemäß der Flow-Matrix. | Konnektivitätstests (ping, traceroute) zwischen VLANs. |
| Die Windows Server-Domäne ist betriebsbereit (AD, DNS, DHCP). | Screenshot von `Active Directory Users and Computers`. |
| Windows/Linux-Clients sind in die Domäne eingebunden. | Screenshot von `nslookup` und `ping` zum DC. |
| Zabbix überwacht mindestens 3 VMs. | Screenshot des Zabbix-Dashboards. |
| Ein verifiziertes Backup wurde wiederhergestellt. | Screenshot des Wiederherstellungsprozesses und Funktionstests. |

## 5. Einschränkungen und Annahmen

### 5.1 Einschränkungen

- **Hardware:** Ein einzelner Laptop (Medion AVANTUN 15E1: AMD Ryzen 5 7430U, 32 GB RAM, 1 TB NVMe).
- **Budget:** Null Euro (alle Software ist Open-Source oder kostenlose Evaluierung).
- **Zeit:** Das Projekt wird außerhalb der Arbeitszeit entwickelt (Wochenenden).
- **Kenntnisse:** Der Autor hat 12 Jahre Erfahrung in kritischen Infrastrukturen, aber dies ist sein erstes Homelab mit verschachteltem Proxmox.

### 5.2 Annahmen

- Die Hardware unterstützt verschachtelte Virtualisierung (AMD-V/SVM im BIOS aktiviert).
- Die Evaluierungsversion von Windows Server 2025 ist für das Labor ausreichend.
- Der physische Router erlaubt die Erstellung einer Bridge ohne Einschränkungen.
- Die Internetverbindung ist stabil für den Download von ISOs und Updates.

## 6. Identifizierte Risiken

| Risiko | Auswirkung | Gegenmaßnahme |
| :--- | :--- | :--- |
| Komplexität der verschachtelten Virtualisierung | Mittel | Validierung von KVM und `/dev/kvm` vor der Proxmox-Bereitstellung (M1). |
| Segmentierungsfehler | Hoch | Flow-Matrix dokumentieren, Konnektivität pro VLAN testen, Default-Deny. |
| Konfigurationsverlust / Drift | Hoch | Versionierte Backups, Snapshots, verifizierte Wiederherstellung (M5). |
| Single Point of Failure (ein Host) | Mittel | Bewusste Akzeptanz in v1; HA-Bewertung in v2. |
| Scope-Drift | Mittel | Roadmap mit geschlossenen Abnahmekriterien. |

## 7. Deliverables pro Meilenstein

| Meilenstein | Deliverable |
| :--- | :--- |
| M0 | README, Umfang, Architektur, ADRs, IP-Plan und Flow-Matrix. |
| M1 | KVM/QEMU-Host betriebsbereit und validiert. |
| M2 | Proxmox VE bereitgestellt und zugänglich. |
| M3 | OPNsense mit VLANs und Firewall-Regeln konfiguriert. |
| M4 | Windows Server-Domäne und Linux/Windows-Clients integriert. |
| M5 | Monitoring (Zabbix) und verifiziertes Backup. |

## 8. Referenzen

- [`README.md`](../README.md) — Hauptprojektdokument.
- [`docs/architecture/`](../architecture/) — Diagramme und Netzwerkpläne.
- [`docs/adr/`](../adr/) — Architekturentscheidungen.
- [`docs/validation/`](../validation/) — Validierungsnachweise.