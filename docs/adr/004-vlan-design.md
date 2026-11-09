[Deutsch](004-vlan-design.md) | [English](004-vlan-design.en.md) | [Español](004-vlan-design.es.md)

# ADR 004 — VLAN-Design und VLSM-Adressierung

- **Status:** Akzeptiert
- **Meilenstein:** M0
- **Datum:** 2026-11-05

## Kontext

Das Labor muss mehrere Workloads mit unterschiedlichen Kritikalitäts- und Vertrauensstufen hosten: Infrastruktur-Server (Windows Server, Debian), Testclients (Windows 11, Debian), eine Perimeter-Firewall (OPNsense) und das Hypervisor-Management (Proxmox VE). Ohne Segmentierung würde der gesamte Datenverkehr eine einzige Broadcast-Domäne teilen, was die Angriffsfläche vergrößern, die Anwendung von Sicherheitsrichtlinien erschweren und die Simulation einer realen Unternehmensumgebung unmöglich machen würde.

Die verfügbare Hardware ist ein einzelner Laptop mit 32 GB RAM, was die Anzahl der VMs begrenzt und eine effiziente Nutzung der IP-Adressen erfordert.

## Entscheidung

Es wird eine **VLAN-basierte Segmentierung** mit **VLSM-Adressierung (Variable Length Subnet Mask)** implementiert, wobei jedem Bereich je nach erwarteter Hostanzahl entsprechend dimensionierte Subnetze zugewiesen werden. Die Segmentierung wird zentral auf der Firewall **FW01 (VM 500)** verwaltet, die als Standard-Gateway für alle VLANs fungiert.

Die definierten VLANs sind:

| VLAN | Name | Zweck | Subnetz |
| :--- | :--- | :--- | :--- |
| 13 | Proxmox-Management | Management des L1-Hypervisors | `10.11.13.0/30` |
| 14 | Virtuelle Server | Infrastruktur-Server | `10.11.14.0/29` |
| 7 | Virtuelle normale Clients | Standard-Testclients | `10.11.7.0/28` |
| 3 | Virtuelle kritische Clients | Kritische Testclients | `10.11.3.0/29` |
| 17 | Admins | Administrationsnetzwerk (Jump Host) | `10.11.17.0/30` |
| 21 | Firewall-Management | Isoliertes Firewall-Management | `10.11.21.0/30` |

## Betrachtete Alternativen

- **Flaches Netzwerk (ohne VLANs):**
  - *Vorteil:* Triviale Konfiguration, keine routingfähige Firewall erforderlich.
  - *Nachteil:* Keine Isolation zwischen den Bereichen; ein kompromittierter Client könnte auf Server oder das Proxmox-Management zugreifen; repräsentiert keine reale Unternehmensumgebung; Default-Deny nicht anwendbar.
- **/24-Subnetze für alle VLANs:**
  - *Vorteil:* Einfach zu berechnen und zu merken.
  - *Nachteil:* Massive IP-Verschwendung (254 Hosts pro VLAN, obwohl nur 2-6 benötigt werden); zeigt keine Subnetting-Kenntnisse; wendet das Least-Privilege-Prinzip nicht an.
- **Physische Segmentierung (mehrere Switches/Router):**
  - *Vorteil:* Echte Isolation auf physischer Ebene.
  - *Nachteil:* In einem Labor mit einem einzigen Laptop nicht realisierbar; kostspielig; nicht skalierbar.

## Konsequenzen

**Positiv:**
- Echte Isolation zwischen den Bereichen: Ein kompromittierter Client kann weder auf das Proxmox-Management noch auf kritische Server zugreifen.
- Anwendung des Least-Privilege-Prinzips: Jedes VLAN hat nur die IPs, die es benötigt.
- Effiziente Nutzung des Adressraums (VLSM): IP-Verschwendung wird vermieden.
- Simulation einer realen Unternehmensumgebung: VLAN-Segmentierung ist der Standard im deutschen Mittelstand.
- Erleichtert die Anwendung der Flow-Matrix (`flow-matrix.md`) mit Default-Deny.

**Negativ:**
- Höhere Konfigurationskomplexität: Erfordert eine Firewall, die zwischen VLANs routet, und eine VLAN-Aware Bridge auf Proxmox.
- Erfordert eine sorgfältige Planung der IP-Adressierung, um Überschneidungen zu vermeiden.
- Die Fehlerbehebung im Netzwerk ist komplexer (Routen, Gateways und Firewall-Regeln müssen überprüft werden).
- Die Laptop-Hardware begrenzt die Anzahl der VMs, was wiederum die Anzahl der Hosts pro VLAN begrenzt.

## Referenzen

- [RFC 1918 — Address Allocation for Private Internets](https://datatracker.ietf.org/doc/html/rfc1918)
- [RFC 3022 — Traditional IP Network Address Translator (NAT)](https://datatracker.ietf.org/doc/html/rfc3022)
- [`docs/architecture/ip-plan.md`](../architecture/ip-plan.md) — Detaillierter IP-Adressierungsplan.
- [`docs/architecture/flow-matrix.md`](../architecture/flow-matrix.md) — Flow-Matrix, die die Segmentierung implementiert.
- [`docs/architecture/vlan-plan-v1.png`](../architecture/vlan-plan-v1.png) — Visuelles Diagramm des VLAN-Plans.