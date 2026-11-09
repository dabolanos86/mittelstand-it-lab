[Deutsch](ip-plan.md) | [English](ip-plan.en.md) | [Español](ip-plan.es.md)

# IP-Adressierungsplan und logische Topologie

**Projekt:** Mittelstand-IT-Lab  
**Dokument:** IP Addressing Plan & Network Topology  
**Version:** 1.0  
**Meilenstein:** M0 - Kickoff & Planung  
**Status:** Vorgeschlagen  
**Datum:** 2026-11-05  

## 1. Zusammenfassung
Dieses Dokument definiert die IPv4-Adresszuweisung, die VLAN-Segmentierung und die logische Topologie der virtualisierten Infrastruktur. Das Design wendet das **Least-Privilege**-Prinzip durch die Verwendung von VLSM (Variable Length Subnet Mask) an und trennt strikt den Management-, Server-, normalen Client-, kritischen Client- und Administrationsverkehr.

Die gesamte interne Segmentierung wird von der virtuellen Maschine **FW01 (VM 500)** verwaltet und geroutet, die als Perimeter-Firewall und Standard-Gateway für alle internen Subnetze fungiert.

## 2. Netzwerk- und VLAN-Tabelle (L2/L3)

| Netzwerk / Beschreibung | VLAN | Subnetz / Basis-IP | Maske | Gateway (FW01) | Notizen / Zweck |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **WAN / Internet** | *N/A* | `192.168.1.0/24` | `255.255.255.0` | `192.168.1.1` | Heimnetzwerk (ISP-Router). Firewall-WAN-Schnittstelle: `.50` |
| **Firewall-Management**| 21 | `10.11.21.0/30` | `255.255.255.252` | `10.11.21.1` | Isoliertes Firewall-Management (FW01). |
| **Proxmox-Management** | 13 | `10.11.13.0/30` | `255.255.255.252` | `10.11.13.1` | Management-Schnittstelle des L1-Hypervisors (Proxmox VE). IP: `.2` |
| **Virtuelle Server** | 14 | `10.11.14.0/29` | `255.255.255.248` | `10.11.14.1` | Infrastruktur-Server (DC01, DC02, MON01). |
| **Virtuelle normale Clients** | 7 | `10.11.7.0/28` | `255.255.255.240` | `10.11.7.1` | Standard-Testclients (Client01). |
| **Virtuelle kritische Clients**| 3 | `10.11.3.0/29` | `255.255.255.248` | `10.11.3.1` | Kritische Testclients / zukünftige Dienste (Client02). |
| **Admins** | 17 | `10.11.17.0/30` | `255.255.255.252` | `10.11.17.1` | Netzwerk für Jump Host / SSH/RDP-Management vom physischen Host. |

## 3. Inventar der virtuellen Maschinen (L2) und IP-Zuweisung

| VM-ID | Hostname | Rolle | OS | VLAN | Zugewiesene IP | Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **VM 100** | `DC01` | AD, DHCP, DNS (Primär) | Windows Server 2025 | 14 | `10.11.14.10` | `10.11.14.1` |
| **VM 101** | `DC02` | AD, DHCP, DNS (Sekundär) | Windows Server 2025 | 14 | `10.11.14.11` | `10.11.14.1` |
| **VM 102** | `MON01` | Monitoring (Zabbix) | Debian 13 | 14 | `10.11.14.13` | `10.11.14.1` |
| **VM 200** | `Client01` | Standard-Client (Tests) | Windows 11 | 7 | `10.11.7.5` | `10.11.7.1` |
| **VM 201** | `Client02` | Kritischer Client (Tests) | Debian 13 | 3 | `10.11.3.5` | `10.11.3.1` |
| **VM 500** | `FW01` | Perimeter-Firewall / Router | OPNsense 26.7 | 21, 13, 14, 7, 3 | Siehe Schnittstellenhinweis | *N/A* |

## 4. Firewall-Schnittstellentopologie (VM 500)

Die Firewall-VM (`FW01`) ist der zentrale Routing-Knoten. Ihre virtuellen Schnittstellen (vNICs) werden wie folgt konfiguriert:

| Virtuelle Schnittstelle | Typ | VLAN | IP / Maske | Beschreibung |
| :--- | :--- | :--- | :--- | :--- |
| `vtnet0` / `em0` | WAN | *N/A* | `192.168.1.50/24` | Internet-Uplink (Bridge `vmbr0` auf L0-Host). |
| `vtnet1` / `em1` | LAN | 14 | `10.11.14.1/29` | Server-Gateway (DC01, DC02, MON01). |
| `vtnet2` / `em2` | LAN | 7 | `10.11.7.1/28` | Gateway für normale Clients. |
| `vtnet3` / `em3` | LAN | 3 | `10.11.3.1/29` | Gateway für kritische Clients. |
| `vtnet4` / `em4` | LAN | 13 | `10.11.13.1/30` | Proxmox-Management-Gateway. |
| `vtnet5` / `em5` | LAN | 21 | `10.11.21.1/30` | Management-Schnittstelle der Firewall selbst. |
| `vtnet6` / `em6` | LAN | 17 | `10.11.17.1/30` | Administrations-Gateway. |

## 5. Implementierungshinweise (M1 / M2)

1.  **Bridge auf dem physischen Host (L0):** Die Datei `/etc/netplan/00-installer-config.yaml` auf dem Ubuntu-Host muss eine Bridge (`vmbr0`) mit der IP `192.168.1.37/24` konfigurieren, um den WAN-Uplink der Firewall bereitzustellen und den Host-Zugriff zu ermöglichen.
2.  **VLAN-Aware Bridge auf Proxmox (L1):** Die interne Proxmox-Bridge muss als *VLAN-Aware* konfiguriert sein, damit die Firewall den Datenverkehr der verschiedenen Subnetze (14, 7, 3 usw.) taggen kann, ohne für jedes VLAN eine physische Bridge erstellen zu müssen.
3.  **Default-Deny:** Die anfängliche Firewall-Richtlinie (`FW01`) auf allen LAN-Schnittstellen besteht darin, **den gesamten Datenverkehr zu verweigern** und dann die in der Datei `flow-matrix.md` dokumentierten Ausnahmen anzuwenden.
4.  **WAN-IP-Reservierung:** Die IP `192.168.1.50` muss außerhalb des DHCP-Pools des physischen Routers (ISP) reserviert werden, um sicherzustellen, dass die Firewall immer dieselbe Adresse im WAN erhält.