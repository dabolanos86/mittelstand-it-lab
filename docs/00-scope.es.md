[Deutsch](00-scope.md) | [English](00-scope.en.md) | [Español](00-scope.es.md)
# Documento de Alcance y Objetivos — Mittelstand-IT-Lab

**Documento:** 00 - Scope & Objectives  
**Versión:** 1.0  
**Hito:** M0 - Kickoff & Planning  
**Estado:** Propuesto  
**Autor:** Ing. Daniel Alejandro Bolanos Solana  
**Fecha:** 2026-11-10  

## 1. Propósito del Proyecto

Mittelstand-IT-Lab es un laboratorio de infraestructura personal diseñado para demostrar competencias prácticas en administración de sistemas Windows/Linux, virtualización anidada, segmentación de red y seguridad de infraestructura IT, con un enfoque orientado a las necesidades reales de la mediana empresa alemana (Mittelstand).

El laboratorio nace de **12 años operando infraestructuras críticas** (aeropuerto internacional, banca). Por eso prioriza la **recuperación verificada**, la **segmentación de mínimo privilegio** y la **trazabilidad auditable**, no solo un entorno de pruebas funcional.

## 2. Alcance del Proyecto

### 2.1 Dentro del alcance (v1)

| Componente | Descripción |
| :--- | :--- |
| **Host físico (L0)** | Ubuntu 26.04.1 LTS con KVM/QEMU y libvirt como capa de virtualización. |
| **Plataforma (L1)** | Proxmox VE 9.2-1 desplegado como VM sobre la capa L0. |
| **Borde (L2)** | OPNsense 26.7 como firewall perimetral y motor de segmentación. |
| **Cargas Windows** | Windows Server 2025 (DC01, DC02) con roles AD, DHCP y DNS. |
| **Cargas Linux** | Debian 13 (MON01 para Zabbix, Client02 para pruebas críticas). |
| **Clientes** | Windows 11 (Client01) y Debian 13 (Client02). |
| **Red** | Segmentación mediante VLANs y direccionamiento IPv4 planificado (VLSM). |
| **Operación** | Monitorización (Zabbix) y estrategia de backup con criterios de recuperación. |

### 2.2 Fuera del alcance (v1)

| Elemento | Razón de exclusión |
| :--- | :--- |
| Exposición a Internet de servicios productivos | El laboratorio es de uso personal; no se expondrán servicios a Internet. |
| Alta disponibilidad multi-nodo (HA) | El hardware es un único portátil; se evaluará en v2. |
| Integración con proveedores cloud | No es necesario para los objetivos de aprendizaje. |
| Licencias retail de Windows Server | Se usará la versión de evaluación oficial (180 días, renovable). |
| Soporte 24/7 | El laboratorio no es un entorno de producción. |

## 3. Objetivos del Proyecto

### 3.1 Objetivos Técnicos

1.  Validar la virtualización anidada (KVM dentro de KVM) en un host Ubuntu.
2.  Desplegar Proxmox VE como hipervisor de nivel 1 sobre la capa L0.
3.  Implementar un firewall perimetral (OPNsense) con segmentación de VLANs.
4.  Desplegar un dominio Windows Server (Active Directory, DNS, DHCP).
5.  Integrar servidores Linux (Debian) y clientes Windows/Linux en el dominio.
6.  Implementar monitorización (Zabbix) y una estrategia de backup verificable.

### 3.2 Objetivos de Aprendizaje

1.  Documentar cada decisión técnica mediante ADRs (Architecture Decision Records).
2.  Mantener un historial de commits atómico y descriptivo.
3.  Generar evidencias de validación en `docs/validation/` para cada hito.
4.  Aplicar el principio de **Default-Deny** en la política de firewall.
5.  Practicar la administración de sistemas Windows y Linux en un entorno segmentado.

## 4. Criterios de Éxito

El proyecto se considerará exitoso si al final del M5 se cumplen los siguientes criterios:

| Criterio | Evidencia |
| :--- | :--- |
| El host L0 tiene KVM/QEMU operativo y `/dev/kvm` accesible. | Captura de `egrep -c '(vmx|svm)' /proc/cpuinfo` y `virsh list`. |
| Proxmox VE es accesible por web y gestiona las VMs. | Captura del panel de Proxmox. |
| OPNsense segmenta el tráfico según la matriz de flujos. | Pruebas de conectividad (ping, traceroute) entre VLANs. |
| El dominio Windows Server está operativo (AD, DNS, DHCP). | Captura de `Active Directory Users and Computers`. |
| Los clientes Windows/Linux están unidos al dominio. | Captura de `nslookup` y `ping` al DC. |
| Zabbix monitoriza al menos 3 VMs. | Captura del dashboard de Zabbix. |
| Se ha restaurado un backup verificado. | Captura del proceso de restauración y prueba de funcionamiento. |

## 5. Restricciones y Supuestos

### 5.1 Restricciones

- **Hardware:** Un único portátil (Medion AVANTUN 15E1: AMD Ryzen 5 7430U, 32 GB RAM, 1 TB NVMe).
- **Presupuesto:** Cero euros (todo el software es open-source o de evaluación gratuita).
- **Tiempo:** El proyecto se desarrolla en horario no laboral (fines de semana).
- **Conocimiento:** El autor tiene 12 años de experiencia en infraestructuras críticas, pero es su primer homelab con Proxmox anidado.

### 5.2 Supuestos

- El hardware soporta virtualización anidada (AMD-V/SVM habilitado en BIOS).
- La versión de evaluación de Windows Server 2025 es suficiente para el laboratorio.
- El router físico permite la creación de un bridge sin restricciones.
- La conexión a Internet es estable para descargar ISOs y actualizaciones.

## 6. Riesgos Identificados

| Riesgo | Impacto | Mitigación |
| :--- | :--- | :--- |
| Complejidad de virtualización anidada | Medio | Validar KVM y `/dev/kvm` antes de desplegar Proxmox (M1). |
| Errores de segmentación | Alto | Documentar matriz de flujos, probar conectividad por VLAN, Default-Deny. |
| Pérdida de configuración / deriva | Alto | Backup versionado, snapshots, restauración verificada (M5). |
| Punto único de fallo (un host) | Medio | Aceptación consciente en v1; evaluación de HA en v2. |
| Deriva de alcance | Medio | Roadmap con criterios de aceptación cerrados. |

## 7. Entregables por Hito

| Hito | Entregable |
| :--- | :--- |
| M0 | README, alcance, arquitectura, ADRs, plan de IPs y matriz de flujos. |
| M1 | Host KVM/QEMU operativo y validado. |
| M2 | Proxmox VE desplegado y accesible. |
| M3 | OPNsense configurado con VLANs y reglas de firewall. |
| M4 | Dominio Windows Server y clientes Linux/Windows integrados. |
| M5 | Monitorización (Zabbix) y backup verificado. |

## 8. Referencias

- [`README.es.md`](../README.es.md) — Documento principal del proyecto.
- [`docs/architecture/`](../architecture/) — Diagramas y planes de red.
- [`docs/adr/`](../adr/) — Decisiones de arquitectura.
- [`docs/validation/`](../validation/) — Evidencias de validación.