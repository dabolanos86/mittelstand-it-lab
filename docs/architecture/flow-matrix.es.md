[Deutsch](flow-matrix.md) | [English](flow-matrix.en.md) | [Español](flow-matrix.es.md)

# Matriz de Flujos de Red y Políticas de Firewall

**Proyecto:** Mittelstand-IT-Lab  
**Documento:** Network Flow Matrix & Firewall Policy  
**Versión:** 1.0  
**Hito:** M0 - Kickoff & Planning  
**Estado:** Proposed  
**Fecha:** 2026-11-05  

## 1. Resumen Ejecutivo

Este documento define la política de seguridad de la red del laboratorio, estableciendo qué tráfico está permitido y cuál debe ser bloqueado entre las diferentes VLANs y hacia Internet. La política se basa en el principio de **Default-Deny (Denegar por defecto)**: todo el tráfico está bloqueado a menos que exista una regla explícita que lo permita.

El firewall perimetral **FW01 (VM 500)** es el único punto de control entre las zonas de confianza (servidores, clientes, gestión) y la zona no confiable (WAN/Internet). Ningún servicio interno se expone directamente a Internet sin pasar por las reglas de NAT y filtrado del firewall.

## 2. Filosofía de Seguridad (Zero Trust)

La segmentación de red se basa en el modelo **Zero Trust**, donde ninguna zona confía en otra por defecto. Cada flujo de tráfico debe ser justificado, documentado y aprobado explícitamente.

| Zona | VLAN | Nivel de Confianza | Descripción |
| :--- | :--- | :--- | :--- |
| **WAN / Internet** | *N/A* | No confiable | Red física doméstica (ISP Router). |
| **Admins** | 17 | Máxima | Red de administración (Jump Host). |
| **Proxmox Management** | 13 | Alta | Gestión del hipervisor L1. |
| **Firewall Management** | 21 | Alta | Gestión aislada del firewall. |
| **Virtual Servers** | 14 | Media | Servidores de infraestructura (AD, DNS, Zabbix). |
| **Virtual Critical Clients** | 3 | Media-Baja | Clientes de pruebas críticas. |
| **Virtual Normal Clients** | 7 | Baja | Clientes estándar de pruebas. |

## 3. Matriz de Flujos Permitidos (Reglas del Firewall)

La siguiente tabla detalla las **excepciones** a la política Default-Deny. Cualquier flujo no listado aquí será descartado por el firewall (Drop/Reject).

| # | Origen (Source) | Destino (Destination) | Puerto / Protocolo | Acción | Justificación / Propósito |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | **VLAN 7** (Normal Clients) | **VLAN 14** (Servers) | TCP/UDP 53 (DNS) | **Permitir** | Resolución de nombres DNS a través de DC01/DC02. |
| 2 | **VLAN 7** (Normal Clients) | **VLAN 14** (Servers) | TCP 88 (Kerberos), TCP 389 (LDAP) | **Permitir** | Autenticación de dominio (Active Directory). |
| 3 | **VLAN 7** (Normal Clients) | **VLAN 14** (Servers) | TCP 445 (SMB) | **Permitir** | Acceso a recursos compartidos de archivos (solo servidores específicos). |
| 4 | **VLAN 14** (Servers) | **WAN** (Internet) | TCP 443 (HTTPS), TCP 80 (HTTP) | **Permitir** | Actualizaciones de sistema operativo y parches de seguridad. |
| 5 | **VLAN 14** (Servers) | **WAN** (Internet) | TCP/UDP 53 (DNS) | **Permitir** | Resolución DNS externa (solo si los DCs no cubren todo). |
| 6 | **VLAN 17** (Admins) | **Todas las VLANs** | TCP 22 (SSH), TCP 3389 (RDP), TCP 443 (HTTPS) | **Permitir** | Acceso de administración a servidores, VMs y Proxmox. |
| 7 | **VLAN 13** (Proxmox Mgmt) | **WAN** (Internet) | TCP 443 (HTTPS) | **Permitir** | Acceso a la interfaz web de Proxmox desde la red doméstica (solo IP estática). |
| 8 | **VLAN 3** (Critical Clients) | **WAN** (Internet) | TCP 443 (HTTPS) | **Permitir** | Actualizaciones del cliente crítico (Debian). |
| 9 | **WAN** (Internet) | **VM 500 (FW01)** | TCP 443 (HTTPS), UDP 51820 (WireGuard) | **Permitir** | Acceso remoto a la gestión del firewall (VPN) y a la interfaz web. |

## 4. Matriz de Flujos Denegados (Reglas Explícitas de Aislamiento)

Para evitar fugas de información y movimiento lateral en caso de compromiso, se aplicarán las siguientes reglas de bloqueo explícitas (además del Default-Deny global):

| Origen | Destino | Acción | Motivo del Bloqueo |
| :--- | :--- | :--- | :--- |
| **VLAN 7** (Normal Clients) | **VLAN 13** (Proxmox Mgmt) | **Denegar** | Aislamiento del hipervisor: los clientes no deben ver ni tocar la gestión de Proxmox. |
| **VLAN 7** (Normal Clients) | **VLAN 3** (Critical Clients) | **Denegar** | Aislamiento de zonas: los clientes normales no deben comunicarse con los críticos. |
| **VLAN 3** (Critical Clients) | **VLAN 7** (Normal Clients) | **Denegar** | Aislamiento de zonas: los críticos no inician conexiones hacia la red de usuarios. |
| **VLAN 7** (Normal Clients) | **WAN** (Internet) | **Denegar** | Los clientes de prueba NO tienen salida directa a Internet. Todo pasa por el DC. |
| **VLAN 14** (Servers) | **VLAN 7 / VLAN 3** | **Denegar** | Los servidores no inician conexiones hacia los clientes (solo responden). |
| **WAN** (Internet) | **VLAN 13, 14, 7, 3** | **Denegar** | Ningún servicio interno debe estar expuesto directamente a Internet (sin NAT/Port Forward). |

## 5. Consideraciones de Implementación (M3)

1.  **NAT (Network Address Translation):** El firewall (`FW01`) aplicará NAT de origen (SNAT) para todo el tráfico que vaya de las VLANs internas hacia la WAN. Las IPs internas (`10.11.x.x`) nunca serán visibles en Internet.
2.  **Logging:** Todas las reglas de **Denegar** deben tener habilitado el registro (Log) para auditar intentos de intrusión o errores de configuración. Las reglas de Permitir solo se registrarán si es estrictamente necesario (para no saturar los discos del firewall).
3.  **Mantenimiento:** Esta matriz se revisará al final de cada hito (M3, M4, M5). Si se añade una nueva VM o servicio, se debe actualizar este documento **antes** de tocar la configuración del firewall.
4.  **Alias y Grupos:** En OPNsense, se recomienda crear **Aliases** (ej. `NET_SERVERS`, `NET_CLIENTS`) y **Groups** para simplificar las reglas. En lugar de escribir una regla por cada IP, se usan los alias, lo que facilita el mantenimiento y reduce errores.
5.  **VPN de Acceso Remoto:** La regla 9 asume una VPN WireGuard para acceso remoto. Si no se implementa en v1, esta regla debe eliminarse o deshabilitarse hasta que la VPN esté configurada.