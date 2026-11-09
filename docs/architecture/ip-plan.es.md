[Deutsch](ip-plan.md) | [English](ip-plan.en.md) | [Español](ip-plan.es.md)
# Plan de Direccionamiento IP y Topología Lógica

**Proyecto:** Mittelstand-IT-Lab  
**Documento:** IP Addressing Plan & Network Topology  
**Versión:** 1.0  
**Hito:** M0 - Kickoff & Planning  
**Estado:** Proposed  
**Fecha:** 2026-11-05  

## 1. Resumen Ejecutivo
Este documento define la asignación de direccionamiento IPv4, la segmentación de VLANs y la topología lógica de la infraestructura virtualizada. El diseño aplica el principio de **mínimo privilegio** mediante el uso de VLSM (Variable Length Subnet Mask), separando estrictamente el tráfico de gestión, servidores, clientes normales, clientes críticos y la red de administración.

Toda la segmentación interna es gestionada y enrutada por la máquina virtual **FW01 (VM 500)**, que actúa como firewall perimetral y default gateway de todas las subredes internas.

## 2. Tabla de Redes y VLANs (L2/L3)

| Red / Descripción | VLAN | Subred / IP Base | Máscara | Gateway (FW01) | Notas / Propósito |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **WAN / Internet** | *N/A* | `192.168.1.0/24` | `255.255.255.0` | `192.168.1.1` | Red física doméstica (ISP Router). Interfaz WAN del Firewall: `.50` |
| **Firewall Management**| 21 | `10.11.21.0/30` | `255.255.255.252` | `10.11.21.1` | Gestión aislada del Firewall (FW01). |
| **Proxmox Management** | 13 | `10.11.13.0/30` | `255.255.255.252` | `10.11.13.1` | Interfaz de gestión del hipervisor L1 (Proxmox VE). IP: `.2` |
| **Virtual Servers** | 14 | `10.11.14.0/29` | `255.255.255.248` | `10.11.14.1` | Servidores de infraestructura (DC01, DC02, MON01). |
| **Virtual Normal Clients** | 7 | `10.11.7.0/28` | `255.255.255.240` | `10.11.7.1` | Clientes estándar de pruebas (Client01). |
| **Virtual Critical Clients**| 3 | `10.11.3.0/29` | `255.255.255.248` | `10.11.3.1` | Clientes de pruebas críticas / futuros servicios (Client02). |
| **Admins** | 17 | `10.11.17.0/30` | `255.255.255.252` | `10.11.17.1` | Red para Jump Host / Gestión SSH/RDP desde el host físico. |

## 3. Inventario de Máquinas Virtuales (L2) y Asignación de IP

| VM ID | Hostname | Rol | SO | VLAN | IP Asignada | Gateway |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| **VM 100** | `DC01` | AD, DHCP, DNS (Primary) | Windows Server 2025 | 14 | `10.11.14.10` | `10.11.14.1` |
| **VM 101** | `DC02` | AD, DHCP, DNS (Secondary) | Windows Server 2025 | 14 | `10.11.14.11` | `10.11.14.1` |
| **VM 102** | `MON01` | Monitorización (Zabbix) | Debian 13 | 14 | `10.11.14.13` | `10.11.14.1` |
| **VM 200** | `Client01` | Cliente estándar (Pruebas) | Windows 11 | 7 | `10.11.7.5` | `10.11.7.1` |
| **VM 201** | `Client02` | Cliente crítico (Pruebas) | Debian 13 | 3 | `10.11.3.5` | `10.11.3.1` |
| **VM 500** | `FW01` | Firewall Perimetral / Router | OPNsense 26.7 | 21, 13, 14, 7, 3 | Ver nota de interfaces | *N/A* |

## 4. Topología de Interfaces del Firewall (VM 500)

La VM del Firewall (`FW01`) es el nodo central de enrutamiento. Sus interfaces virtuales (vNICs) se configurarán de la siguiente manera:

| Interfaz Virtual | Tipo | VLAN | IP / Máscara | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `vtnet0` / `em0` | WAN | *N/A* | `192.168.1.50/24` | Salida a Internet (Bridge `vmbr0` en Host L0). |
| `vtnet1` / `em1` | LAN | 14 | `10.11.14.1/29` | Gateway de Servidores (DC01, DC02, MON01). |
| `vtnet2` / `em2` | LAN | 7 | `10.11.7.1/28` | Gateway de Clientes Normales. |
| `vtnet3` / `em3` | LAN | 3 | `10.11.3.1/29` | Gateway de Clientes Críticos. |
| `vtnet4` / `em4` | LAN | 13 | `10.11.13.1/30` | Gateway de Gestión de Proxmox. |
| `vtnet5` / `em5` | LAN | 21 | `10.11.21.1/30` | Interfaz de Gestión del propio Firewall. |
| `vtnet6` / `em6` | LAN | 17 | `10.11.17.1/30` | Gateway de Administración. |

## 5. Consideraciones de Implementación (M1 / M2)

1.  **Bridge en el Host Físico (L0):** El archivo `/etc/netplan/00-installer-config.yaml` del host Ubuntu deberá configurar un bridge (`vmbr0`) con la IP `192.168.1.37/24` para dar salida a la WAN del Firewall y permitir el acceso al host.
2.  **VLAN-Aware Bridge en Proxmox (L1):** El bridge interno de Proxmox debe estar configurado como *VLAN-Aware* para que el Firewall pueda etiquetar el tráfico de las diferentes subredes (14, 7, 3, etc.) sin necesidad de crear un bridge físico por cada VLAN.
3.  **Default-Deny:** La política inicial del firewall (`FW01`) en todas las interfaces LAN será **denegar todo el tráfico** y luego aplicar las excepciones documentadas en el archivo `flow-matrix.md`.
4.  **Reserva de IPs WAN:** La IP `192.168.1.50` debe estar reservada fuera del pool DHCP del router físico (ISP) para garantizar que el Firewall siempre reciba la misma dirección en la WAN.