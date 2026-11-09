[Deutsch](002-nested-virtualization.de.md) | [English](002-nested-virtualization.en.md) | [Español](002-nested-virtualization.es.md)

# ADR 002 — Verschachtelte Virtualisierung: Proxmox VE auf KVM/QEMU

- **Status:** Akzeptiert
- **Meilenstein:** M0
- **Datum:** 2026-11-01

## Kontext

Das Labor muss einen Level-1-Hypervisor (Proxmox VE) hosten, um die virtuellen Maschinen des Projekts zu verwalten (Windows/Linux-Server, OPNsense-Firewall, Testclients). Die verfügbare Hardware ist ein einzelner Laptop (Medion AVANTUN 15E1: AMD Ryzen 5 7430U, 32 GB RAM, 1 TB NVMe), der auch als tägliche Workstation mit Ubuntu 24.04 LTS verwendet wird.

Es gibt zwei Hauptoptionen für die Bereitstellung von Proxmox:

1.  **Bare-Metal-Installation:** Proxmox VE direkt auf der physischen Festplatte installieren und Ubuntu entfernen.
2.  **Verschachtelte Virtualisierung:** Ubuntu als Host beibehalten, KVM/QEMU + libvirt installieren und Proxmox VE als virtuelle Maschine (L1) auf dieser Schicht bereitstellen.

## Entscheidung

Es wird die **verschachtelte Virtualisierung** gewählt. Proxmox VE 8.2 wird als L1-VM auf Ubuntu 24.04 LTS (L0) bereitgestellt, wobei KVM/QEMU und libvirt als Virtualisierungs-Engine verwendet werden. Der Proxmox-VM werden 4 vCPUs, 16 GB RAM und 300 GB Festplatte zugewiesen, und sie wird mit der Bridge `vmbr0` verbunden, um Zugriff auf das WAN und die internen VLANs zu haben.

## Betrachtete Alternativen

- **Bare-Metal (Proxmox direkt auf der Hardware):**
  - *Vorteil:* Maximale Leistung, keine Zwischenschicht.
  - *Nachteil:* Entfernt Ubuntu als tägliche Arbeitsumgebung; jeder Proxmox-Fehler macht den Laptop für andere Aufgaben unbrauchbar; schwierig rückgängig zu machen, ohne das Betriebssystem neu zu installieren.
- **VirtualBox / VMware Workstation:**
  - *Vorteil:* Einfach zu bedienen.
  - *Nachteil:* Geringere Leistung, unterstützt KVM-Nesting nicht nativ, nicht repräsentativ für eine Unternehmensumgebung.
- **Container (LXC/Docker):**
  - *Vorteil:* Leichtgewichtig.
  - *Nachteil:* Ermöglicht nicht die Simulation eines Level-1-Hypervisors oder das Testen vollständiger Virtualisierungsszenarien (Windows Server, OPNsense).

## Konsequenzen

**Positiv:**
- Ubuntu bleibt als tägliche Arbeitsumgebung erhalten (IDE, Browser, Tools).
- Der Laptop bleibt auch bei einem Proxmox-Ausfall nutzbar; er kann aus einem Snapshot der L1-VM wiederhergestellt werden.
- Verschachtelte Virtualisierung wird praktiziert, eine in Cloud-Umgebungen und Testlaboren gefragte Fähigkeit.
- Ermöglicht Snapshots des Hypervisors selbst vor riskanten Änderungen.
- Isoliert das Labor vom Host-Betriebssystem.

**Negativ:**
- Leistungseinbuße durch die doppelte Virtualisierungsschicht (geschätzt 10-15%).
- Zusätzliche Komplexität bei der Netzwerkkonfiguration (Bridges, VLAN-Aware Bridges).
- Die Laptop-Hardware (32 GB RAM) begrenzt die Anzahl der gleichzeitig ausführbaren VMs.
- Verschachtelte Virtualisierung erfordert, dass AMD-V (SVM Mode) im BIOS aktiviert ist.

## Referenzen

- [Proxmox VE Documentation — Nested Virtualization](https://pve.proxmox.com/wiki/Nested_Virtualization)
- [KVM — Nested Guests](https://www.linux-kvm.org/page/Nested_Guests)
- `docs/01-host-kvm-audit.md` — *(Ausstehende Erstellung in M1)* Nachweis der Validierung von `/dev/kvm` auf dem L0-Host.