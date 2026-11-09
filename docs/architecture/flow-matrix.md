[Deutsch](flow-matrix.md) | [English](flow-matrix.en.md) | [Español](flow-matrix.es.md)

# Netzwerk-Flow-Matrix und Firewall-Richtlinie

**Projekt:** Mittelstand-IT-Lab  
**Dokument:** Network Flow Matrix & Firewall Policy  
**Version:** 1.0  
**Meilenstein:** M0 - Kickoff & Planung  
**Status:** Vorgeschlagen  
**Datum:** 2026-11-05  

## 1. Zusammenfassung

Dieses Dokument definiert die Netzwerksicherheitsrichtlinie des Labors und legt fest, welcher Datenverkehr zwischen den verschiedenen VLANs und zum Internet erlaubt und welcher blockiert werden muss. Die Richtlinie basiert auf dem **Default-Deny**-Prinzip: Der gesamte Datenverkehr wird blockiert, sofern keine explizite Regel ihn erlaubt.

Die Perimeter-Firewall **FW01 (VM 500)** ist der einzige Kontrollpunkt zwischen den vertrauenswürdigen Zonen (Server, Clients, Management) und der nicht vertrauenswürdigen Zone (WAN/Internet). Kein interner Dienst wird direkt ins Internet exponiert, ohne die NAT- und Filterregeln der Firewall zu durchlaufen.

## 2. Sicherheitsphilosophie (Zero Trust)

Die Netzwerksegmentierung basiert auf dem **Zero-Trust**-Modell, bei dem keine Zone standardmäßig einer anderen vertraut. Jeder Datenfluss muss begründet, dokumentiert und explizit genehmigt werden.

| Zone | VLAN | Vertrauensniveau | Beschreibung |
| :--- | :--- | :--- | :--- |
| **WAN / Internet** | *N/A* | Nicht vertrauenswürdig | Heimnetzwerk (ISP-Router). |
| **Admins** | 17 | Maximal | Administrationsnetzwerk (Jump Host). |
| **Proxmox-Management** | 13 | Hoch | Management des L1-Hypervisors. |
| **Firewall-Management** | 21 | Hoch | Isoliertes Firewall-Management. |
| **Virtuelle Server** | 14 | Mittel | Infrastruktur-Server (AD, DNS, Zabbix). |
| **Virtuelle kritische Clients** | 3 | Mittel-Niedrig | Kritische Testclients. |
| **Virtuelle normale Clients** | 7 | Niedrig | Standard-Testclients. |

## 3. Matrix der erlaubten Flüsse (Firewall-Regeln)

Die folgende Tabelle beschreibt die **Ausnahmen** von der Default-Deny-Richtlinie. Jeder hier nicht aufgeführte Datenfluss wird von der Firewall verworfen (Drop/Reject).

| # | Quelle (Source) | Ziel (Destination) | Port / Protokoll | Aktion | Begründung / Zweck |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | **VLAN 7** (Normale Clients) | **VLAN 14** (Server) | TCP/UDP 53 (DNS) | **Erlauben** | DNS-Namensauflösung über DC01/DC02. |
| 2 | **VLAN 7** (Normale Clients) | **VLAN 14** (Server) | TCP 88 (Kerberos), TCP 389 (LDAP) | **Erlauben** | Domänenauthentifizierung (Active Directory). |
| 3 | **VLAN 7** (Normale Clients) | **VLAN 14** (Server) | TCP 445 (SMB) | **Erlauben** | Zugriff auf Dateifreigaben (nur bestimmte Server). |
| 4 | **VLAN 14** (Server) | **WAN** (Internet) | TCP 443 (HTTPS), TCP 80 (HTTP) | **Erlauben** | Betriebssystem-Updates und Sicherheitspatches. |
| 5 | **VLAN 14** (Server) | **WAN** (Internet) | TCP/UDP 53 (DNS) | **Erlauben** | Externe DNS-Auflösung (nur wenn DCs nicht alles abdecken). |
| 6 | **VLAN 17** (Admins) | **Alle VLANs** | TCP 22 (SSH), TCP 3389 (RDP), TCP 443 (HTTPS) | **Erlauben** | Administrativer Zugriff auf Server, VMs und Proxmox. |
| 7 | **VLAN 13** (Proxmox Mgmt) | **WAN** (Internet) | TCP 443 (HTTPS) | **Erlauben** | Zugriff auf die Proxmox-Weboberfläche aus dem Heimnetz (nur statische IP). |
| 8 | **VLAN 3** (Kritische Clients) | **WAN** (Internet) | TCP 443 (HTTPS) | **Erlauben** | Updates des kritischen Clients (Debian). |
| 9 | **WAN** (Internet) | **VM 500 (FW01)** | TCP 443 (HTTPS), UDP 51820 (WireGuard) | **Erlauben** | Fernzugriff auf das Firewall-Management (VPN) und die Weboberfläche. |

## 4. Matrix der verweigerten Flüsse (Explizite Isolationsregeln)

Um Informationslecks und laterale Bewegung im Kompromittierungsfall zu verhindern, werden die folgenden expliziten Sperrregeln angewendet (zusätzlich zum globalen Default-Deny):

| Quelle | Ziel | Aktion | Grund der Sperrung |
| :--- | :--- | :--- | :--- |
| **VLAN 7** (Normale Clients) | **VLAN 13** (Proxmox Mgmt) | **Verweigern** | Hypervisor-Isolation: Clients dürfen das Proxmox-Management nicht sehen oder berühren. |
| **VLAN 7** (Normale Clients) | **VLAN 3** (Kritische Clients) | **Verweigern** | Zonen-Isolation: Normale Clients dürfen nicht mit kritischen kommunizieren. |
| **VLAN 3** (Kritische Clients) | **VLAN 7** (Normale Clients) | **Verweigern** | Zonen-Isolation: Kritische Clients initiieren keine Verbindungen zum Benutzernetzwerk. |
| **VLAN 7** (Normale Clients) | **WAN** (Internet) | **Verweigern** | Testclients haben KEINEN direkten Internetzugang. Alles läuft über den DC. |
| **VLAN 14** (Server) | **VLAN 7 / VLAN 3** | **Verweigern** | Server initiieren keine Verbindungen zu Clients (sie antworten nur). |
| **WAN** (Internet) | **VLAN 13, 14, 7, 3** | **Verweigern** | Kein interner Dienst darf direkt ins Internet exponiert werden (ohne NAT/Port Forward). |

## 5. Implementierungshinweise (M3)

1.  **NAT (Network Address Translation):** Die Firewall (`FW01`) wendet Source-NAT (SNAT) für den gesamten Datenverkehr von den internen VLANs zum WAN an. Interne IPs (`10.11.x.x`) sind im Internet niemals sichtbar.
2.  **Logging:** Alle **Verweigern**-Regeln müssen die Protokollierung (Log) aktiviert haben, um Eindringversuche oder Fehlkonfigurationen zu auditieren. Erlauben-Regeln werden nur protokolliert, wenn es unbedingt erforderlich ist (um die Festplatten der Firewall nicht zu überlasten).
3.  **Wartung:** Diese Matrix wird am Ende jedes Meilensteins (M3, M4, M5) überprüft. Wenn eine neue VM oder ein neuer Dienst hinzugefügt wird, muss dieses Dokument **vor** der Änderung der Firewall-Konfiguration aktualisiert werden.
4.  **Aliase und Gruppen:** In OPNsense wird empfohlen, **Aliase** (z. B. `NET_SERVERS`, `NET_CLIENTS`) und **Gruppen** zu erstellen, um die Regeln zu vereinfachen. Anstatt eine Regel pro IP zu schreiben, werden Aliase verwendet, was die Wartung erleichtert und Fehler reduziert.
5.  **Remote-Access-VPN:** Regel 9 setzt ein WireGuard-VPN für den Fernzugriff voraus. Wenn es in v1 nicht implementiert wird, muss diese Regel entfernt oder deaktiviert werden, bis das VPN konfiguriert ist.