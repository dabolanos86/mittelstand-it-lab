[Deutsch](003-opnsense-vs-pfsense.md) | [English](003-opnsense-vs-pfsense.en.md) | [Español](003-opnsense-vs-pfsense.es.md)

# ADR 003 — Elección de OPNsense sobre pfSense como firewall perimetral

- **Estado:** Aceptado
- **Hito:** M0
- **Fecha:** 2026-11-02

## Contexto

El laboratorio requiere un firewall perimetral que actúe como puerta de enlace entre la red doméstica (WAN) y las VLANs internas (servidores, clientes, gestión). Este firewall debe soportar segmentación por VLANs, NAT, reglas de filtrado stateful y, preferiblemente, funcionalidades de IDS/IPS y VPN.

Las dos opciones principales, ambas basadas en FreeBSD y con el motor de filtrado `pf`, son **OPNsense** y **pfSense**. Ambas son maduras, open-source y ampliamente utilizadas en entornos empresariales y homelabs.

## Decisión

Se elige **OPNsense 26.7** como firewall perimetral del laboratorio.

## Alternativas consideradas

- **pfSense Community Edition:**
  - *Ventaja:* Base de usuarios empresariales muy amplia, documentación extensa, estabilidad probada durante años.
  - *Desventaja:* Ciclo de actualizaciones más lento, interfaz de usuario menos moderna, algunas funcionalidades avanzadas requieren suscripción de pago (pfSense Plus).
- **OPNsense:**
  - *Ventaja:* Ciclo de actualizaciones más ágil, interfaz de usuario más moderna e intuitiva, sistema de plugins más limpio y centralizado, comunidad activa en foros de Proxmox.
  - *Desventaja:* Menor base de usuarios empresariales que pfSense, aunque en crecimiento.
- **Firewall basado en Linux (nftables/iptables):**
  - *Ventaja:* Control total, ligero.
  - *Desventaja:* Requiere configuración manual extensa, no ofrece interfaz web ni funcionalidades de IDS/IPS integradas, mayor curva de aprendizaje y mantenimiento.

## Consecuencias

**Positivas:**
- Interfaz de usuario más moderna y fácil de usar, lo que reduce la curva de aprendizaje y facilita la documentación con capturas de pantalla.
- Ciclo de actualizaciones más ágil, ideal para un laboratorio donde se quiere probar lo último en seguridad.
- Sistema de plugins centralizado que facilita la incorporación de funcionalidades (IDS/IPS, VPN, proxy).
- Integración sencilla con Proxmox VE como VM, con amplia documentación en la comunidad.
- Alineación con el objetivo del proyecto de demostrar competencias en herramientas modernas de seguridad.

**Negativas:**
- Menor base de usuarios empresariales que pfSense, lo que puede traducirse en menos tutoriales específicos para casos muy concretos.
- Algunos plugins de terceros pueden no estar tan maduros como sus equivalentes en pfSense.
- Requiere revisión manual de las actualizaciones para evitar regresiones en el laboratorio.

## Referencias

- [OPNsense Official Documentation](https://docs.opnsense.org/)
- [pfSense Documentation](https://docs.netgate.com/pfsense/en/latest/)
- [Proxmox VE Wiki — OPNsense](https://pve.proxmox.com/wiki/OPNsense)
- [`docs/architecture/flow-matrix.es.md`](../architecture/flow-matrix.es.md) — Matriz de flujos que implementa este firewall.
- [`docs/architecture/ip-plan.es.md`](../architecture/ip-plan.es.md) — Plan de direccionamiento que asigna las interfaces del firewall.