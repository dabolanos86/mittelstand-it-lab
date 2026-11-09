[Deutsch](002-nested-virtualization.de.md) | [English](002-nested-virtualization.en.md) | [Español](002-nested-virtualization.es.md)

# ADR 002 — Virtualización anidada: Proxmox VE sobre KVM/QEMU

- **Estado:** Aceptado
- **Hito:** M0
- **Fecha:** 2026-11-01

## Contexto

El laboratorio debe alojar un hipervisor de nivel 1 (Proxmox VE) para gestionar las máquinas virtuales del proyecto (servidores Windows/Linux, firewall OPNsense, clientes de prueba). El hardware disponible es un único portátil (Medion AVANTUN 15E1: AMD Ryzen 5 7430U, 32 GB RAM, 1 TB NVMe), que también se utiliza como estación de trabajo diaria con Ubuntu 24.04 LTS.

Existen dos opciones principales para desplegar Proxmox:

1.  **Instalación bare-metal:** Instalar Proxmox VE directamente sobre el disco físico, eliminando Ubuntu.
2.  **Virtualización anidada:** Mantener Ubuntu como host, instalar KVM/QEMU + libvirt, y desplegar Proxmox VE como una máquina virtual (L1) sobre esa capa.

## Decisión

Se elige la **virtualización anidada**. Proxmox VE 8.2 se desplegará como una VM (L1) sobre Ubuntu 24.04 LTS (L0), utilizando KVM/QEMU y libvirt como motor de virtualización. La VM de Proxmox tendrá asignados 4 vCPU, 16 GB RAM y 300 GB de disco, y se conectará al bridge `vmbr0` para tener acceso a la WAN y a las VLANs internas.

## Alternativas consideradas

- **Bare-metal (Proxmox directamente sobre el hardware):**
  - *Ventaja:* Máximo rendimiento, sin capa intermedia.
  - *Desventaja:* Elimina Ubuntu como entorno de trabajo diario; cualquier fallo en Proxmox inutiliza el portátil para otras tareas; difícil de revertir sin reinstalar el sistema operativo.
- **VirtualBox / VMware Workstation:**
  - *Ventaja:* Fácil de usar.
  - *Desventaja:* Menor rendimiento, no soporta anidamiento de KVM de forma nativa, no es representativo de un entorno empresarial.
- **Contenedores (LXC/Docker):**
  - *Ventaja:* Ligero.
  - *Desventaja:* No permite simular un hipervisor de nivel 1 ni probar escenarios de virtualización completa (Windows Server, OPNsense).

## Consecuencias

**Positivas:**
- Se preserva Ubuntu como entorno de trabajo diario (IDE, navegador, herramientas).
- El portátil sigue siendo utilizable aunque Proxmox falle; se puede restaurar desde un snapshot de la VM L1.
- Se practica virtualización anidada, una habilidad demandada en entornos cloud y laboratorios de pruebas.
- Permite tomar snapshots del propio hipervisor antes de cambios arriesgados.
- Aísla el laboratorio del sistema operativo anfitrión.

**Negativas:**
- Penalización de rendimiento por la doble capa de virtualización (estimada en un 10-15%).
- Complejidad adicional en la configuración de red (bridges, VLAN-Aware bridges).
- El hardware del portátil (32 GB RAM) limita el número de VMs que se pueden ejecutar simultáneamente.
- La virtualización anidada requiere que AMD-V (SVM Mode) esté habilitado en la BIOS.

## Referencias

- [Proxmox VE Documentation — Nested Virtualization](https://pve.proxmox.com/wiki/Nested_Virtualization)
- [KVM — Nested Guests](https://www.linux-kvm.org/page/Nested_Guests)
- [`docs/01-host-kvm-audit.md`](../01-host-kvm-audit.md) — *(Pendiente de creación en M1)* Evidencia de la validación de `/dev/kvm` en el host L0.