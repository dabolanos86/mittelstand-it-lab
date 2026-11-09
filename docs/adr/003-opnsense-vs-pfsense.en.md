[Deutsch](003-opnsense-vs-pfsense.md) | [English](003-opnsense-vs-pfsense.en.md) | [Español](003-opnsense-vs-pfsense.es.md)

# ADR 003 — Choosing OPNsense over pfSense as Perimeter Firewall

- **Status:** Accepted
- **Milestone:** M0
- **Date:** 2026-11-02

## Context

The lab requires a perimeter firewall to act as a gateway between the home network (WAN) and the internal VLANs (servers, clients, management). This firewall must support VLAN segmentation, NAT, stateful filtering rules and, preferably, IDS/IPS and VPN features.

The two main options, both based on FreeBSD and using the `pf` filtering engine, are **OPNsense** and **pfSense**. Both are mature, open-source and widely used in enterprise environments and homelabs.

## Decision

**OPNsense 26.7** is chosen as the lab's perimeter firewall.

## Alternatives considered

- **pfSense Community Edition:**
  - *Advantage:* Very large enterprise user base, extensive documentation, years of proven stability.
  - *Disadvantage:* Slower update cycle, less modern user interface, some advanced features require a paid subscription (pfSense Plus).
- **OPNsense:**
  - *Advantage:* More agile update cycle, more modern and intuitive user interface, cleaner and more centralized plugin system, active community in Proxmox forums.
  - *Disadvantage:* Smaller enterprise user base than pfSense, although growing.
- **Linux-based firewall (nftables/iptables):**
  - *Advantage:* Full control, lightweight.
  - *Disadvantage:* Requires extensive manual configuration, no web interface or integrated IDS/IPS features, steeper learning curve and maintenance.

## Consequences

**Positive:**
- More modern and easier-to-use user interface, which reduces the learning curve and facilitates documentation with screenshots.
- More agile update cycle, ideal for a lab where you want to test the latest in security.
- Centralized plugin system that facilitates the addition of features (IDS/IPS, VPN, proxy).
- Simple integration with Proxmox VE as a VM, with extensive community documentation.
- Alignment with the project's goal of demonstrating skills in modern security tools.

**Negative:**
- Smaller enterprise user base than pfSense, which may translate into fewer specific tutorials for very concrete cases.
- Some third-party plugins may not be as mature as their pfSense equivalents.
- Requires manual review of updates to avoid regressions in the lab.

## References

- [OPNsense Official Documentation](https://docs.opnsense.org/)
- [pfSense Documentation](https://docs.netgate.com/pfsense/en/latest/)
- [Proxmox VE Wiki — OPNsense](https://pve.proxmox.com/wiki/OPNsense)
- [`docs/architecture/flow-matrix.en.md`](../architecture/flow-matrix.en.md) — Flow matrix that implements this firewall.
- [`docs/architecture/ip-plan.en.md`](../architecture/ip-plan.en.md) — Addressing plan that assigns the firewall interfaces.