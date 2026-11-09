[Deutsch](004-vlan-design.md) | [English](004-vlan-design.en.md) | [Español](004-vlan-design.es.md)

# ADR 004 — Diseño de VLANs y direccionamiento VLSM

- **Estado:** Aceptado
- **Hito:** M0
- **Fecha:** 2026-11-05

## Contexto

El laboratorio debe alojar múltiples cargas de trabajo con diferentes niveles de criticidad y confianza: servidores de infraestructura (Windows Server, Debian), clientes de prueba (Windows 11, Debian), un firewall perimetral (OPNsense) y la gestión del hipervisor (Proxmox VE). Sin segmentación, todo el tráfico compartiría un único dominio de broadcast, lo que aumentaría la superficie de ataque, dificultaría la aplicación de políticas de seguridad y haría imposible simular un entorno empresarial real.

El hardware disponible es un único portátil con 32 GB de RAM, lo que limita el número de VMs y exige un uso eficiente de las direcciones IP.

## Decisión

Se implementa una **segmentación basada en VLANs** con direccionamiento **VLSM (Variable Length Subnet Mask)**, asignando subredes de tamaño ajustado a cada zona según su número de hosts previsto. La segmentación se gestiona centralizadamente en el firewall **FW01 (VM 500)**, que actúa como default gateway de todas las VLANs.

Las VLANs definidas son:

| VLAN | Nombre | Propósito | Subred |
| :--- | :--- | :--- | :--- |
| 13 | Proxmox Management | Gestión del hipervisor L1 | `10.11.13.0/30` |
| 14 | Virtual Servers | Servidores de infraestructura | `10.11.14.0/29` |
| 7 | Virtual Normal Clients | Clientes estándar de pruebas | `10.11.7.0/28` |
| 3 | Virtual Critical Clients | Clientes de pruebas críticas | `10.11.3.0/29` |
| 17 | Admins | Red de administración (Jump Host) | `10.11.17.0/30` |
| 21 | Firewall Management | Gestión aislada del firewall | `10.11.21.0/30` |

## Alternativas consideradas

- **Red plana (sin VLANs):**
  - *Ventaja:* Configuración trivial, sin necesidad de un firewall que enrute.
  - *Desventaja:* Sin aislamiento entre zonas; un cliente comprometido podría acceder a los servidores o a la gestión de Proxmox; no representa un entorno empresarial real; imposible aplicar Default-Deny.
- **Subredes /24 para todas las VLANs:**
  - *Ventaja:* Fácil de calcular y recordar.
  - *Desventaja:* Desperdicio masivo de IPs (254 hosts por VLAN cuando solo se necesitan 2-6); no demuestra dominio de subnetting; no aplica el principio de mínimo privilegio.
- **Segmentación física (múltiples switches/routers):**
  - *Ventaja:* Aislamiento real a nivel físico.
  - *Desventaja:* Inviable en un laboratorio con un único portátil; costoso; no escalable.

## Consecuencias

**Positivas:**
- Aislamiento real entre zonas: un cliente comprometido no puede acceder a la gestión de Proxmox ni a los servidores críticos.
- Aplicación del principio de mínimo privilegio: cada VLAN solo tiene las IPs que necesita.
- Uso eficiente del espacio de direcciones (VLSM): se evita el desperdicio de IPs.
- Simulación de un entorno empresarial real: la segmentación por VLANs es el estándar en la mediana empresa alemana.
- Facilita la aplicación de la matriz de flujos (`flow-matrix.es.md`) con Default-Deny.

**Negativas:**
- Mayor complejidad de configuración: requiere un firewall que enrute entre VLANs y un bridge VLAN-Aware en Proxmox.
- Requiere planificación cuidadosa del direccionamiento IP para evitar solapamientos.
- La depuración de problemas de red es más compleja (hay que verificar rutas, gateways y reglas de firewall).
- El hardware del portátil limita el número de VMs, lo que a su vez limita el número de hosts por VLAN.

## Referencias

- [RFC 1918 — Address Allocation for Private Internets](https://datatracker.ietf.org/doc/html/rfc1918)
- [RFC 3022 — Traditional IP Network Address Translator (NAT)](https://datatracker.ietf.org/doc/html/rfc3022)
- [`docs/architecture/ip-plan.es.md`](../architecture/ip-plan.es.md) — Plan de direccionamiento IP detallado.
- [`docs/architecture/flow-matrix.es.md`](../architecture/flow-matrix.es.md) — Matriz de flujos que implementa la segmentación.
- [`docs/architecture/vlan-plan-v1.png`](../architecture/vlan-plan-v1.png) — Diagrama visual del plan de VLANs.