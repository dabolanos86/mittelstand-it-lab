[Deutsch](003-opnsense-vs-pfsense.md) | [English](003-opnsense-vs-pfsense.en.md) | [Español](003-opnsense-vs-pfsense.es.md)

# ADR 003 — Wahl von OPNsense gegenüber pfSense als Perimeter-Firewall

- **Status:** Akzeptiert
- **Meilenstein:** M0
- **Datum:** 2026-11-02

## Kontext

Das Labor benötigt eine Perimeter-Firewall, die als Gateway zwischen dem Heimnetzwerk (WAN) und den internen VLANs (Server, Clients, Management) fungiert. Diese Firewall muss VLAN-Segmentierung, NAT, zustandsbehaftete Filterregeln und vorzugsweise IDS/IPS- und VPN-Funktionen unterstützen.

Die beiden Hauptoptionen, beide auf FreeBSD basierend und mit der Filter-Engine `pf`, sind **OPNsense** und **pfSense**. Beide sind ausgereift, Open-Source und in Unternehmensumgebungen und Homelabs weit verbreitet.

## Entscheidung

**OPNsense 26.7** wird als Perimeter-Firewall des Labors gewählt.

## Betrachtete Alternativen

- **pfSense Community Edition:**
  - *Vorteil:* Sehr große Unternehmensbenutzerbasis, umfangreiche Dokumentation, jahrelang bewährte Stabilität.
  - *Nachteil:* Langsamerer Update-Zyklus, weniger moderne Benutzeroberfläche, einige erweiterte Funktionen erfordern ein kostenpflichtiges Abonnement (pfSense Plus).
- **OPNsense:**
  - *Vorteil:* Agilerer Update-Zyklus, modernere und intuitivere Benutzeroberfläche, saubereres und zentralisierteres Plugin-System, aktive Community in Proxmox-Foren.
  - *Nachteil:* Kleinere Unternehmensbenutzerbasis als pfSense, obwohl wachsend.
- **Linux-basierte Firewall (nftables/iptables):**
  - *Vorteil:* Volle Kontrolle, leichtgewichtig.
  - *Nachteil:* Erfordert umfangreiche manuelle Konfiguration, bietet keine Weboberfläche oder integrierte IDS/IPS-Funktionen, steilere Lernkurve und Wartung.

## Konsequenzen

**Positiv:**
- Modernere und benutzerfreundlichere Oberfläche, was die Lernkurve reduziert und die Dokumentation mit Screenshots erleichtert.
- Agilerer Update-Zyklus, ideal für ein Labor, in dem man das Neueste in Sachen Sicherheit testen möchte.
- Zentralisiertes Plugin-System, das die Integration von Funktionen (IDS/IPS, VPN, Proxy) erleichtert.
- Einfache Integration mit Proxmox VE als VM, mit umfangreicher Community-Dokumentation.
- Ausrichtung an das Projektziel, Kompetenzen in modernen Sicherheitswerkzeugen zu demonstrieren.

**Negativ:**
- Kleinere Unternehmensbenutzerbasis als pfSense, was zu weniger spezifischen Tutorials für sehr konkrete Fälle führen kann.
- Einige Drittanbieter-Plugins sind möglicherweise nicht so ausgereift wie ihre pfSense-Äquivalente.
- Erfordert eine manuelle Überprüfung der Updates, um Regressionen im Labor zu vermeiden.

## Referenzen

- [OPNsense Official Documentation](https://docs.opnsense.org/)
- [pfSense Documentation](https://docs.netgate.com/pfsense/en/latest/)
- [Proxmox VE Wiki — OPNsense](https://pve.proxmox.com/wiki/OPNsense)
- [`docs/architecture/flow-matrix.md`](../architecture/flow-matrix.md) — Flow-Matrix, die diese Firewall implementiert.
- [`docs/architecture/ip-plan.md`](../architecture/ip-plan.md) — Adressierungsplan, der die Firewall-Schnittstellen zuweist.