[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

# Arquitectura Lógica y de Red — Mittelstand-IT-Lab

**Documento:** Índice de Arquitectura  
**Versión:** 1.0  
**Hito:** M0 - Kickoff & Planning  
**Estado:** Proposed  
**Fecha:** 2026-11-10  

## 📌 Propósito de este directorio

Este directorio contiene los artefactos de diseño lógico de la infraestructura del laboratorio. Aquí se define **cómo** está construido el sistema (capas de virtualización), **cómo** se comunican los componentes (topología de red) y **qué** direcciones IP y VLANs se utilizarán. 

Toda la documentación aquí presente representa el estado **"As-Designed"** (Diseñado). Las evidencias de que el sistema funciona como se diseñó se encontrarán en `docs/validation/` una vez se ejecuten los hitos de implementación (M1 en adelante).

## 🗂️ Índice de Artefactos

A continuación se detallan los documentos y diagramas disponibles en esta carpeta:

| Artefacto | Tipo | Descripción |
| :--- | :--- | :--- |
| [`architecture-v1.png`](./architecture-v1.png) | Diagrama | Vista jerárquica de la infraestructura (L0 Host Físico, L1 Proxmox Anidado, L2 VMs). |
| [`architecture-v1.drawio`](./architecture-v1.drawio) | Fuente | Archivo editable del diagrama jerárquico. |
| [`vlan-plan-v1.png`](./vlan-plan-v1.png) | Diagrama | Tabla visual del plan de VLANs, subredes (VLSM) y Gateways. |
| [`vlan-plan-v1.drawio`](./vlan-plan-v1.drawio) | Fuente | Archivo editable del plan de VLANs. |
| [`ip-plan.es.md`](./ip-plan.es.md) | Documento | Plan de direccionamiento IP detallado, inventario de VMs e interfaces del Firewall. |
| [`flow-matrix.es.md`](./flow-matrix.es.md) | Documento | Matriz de flujos del Firewall (Default-Deny y excepciones). |

## 🔗 Relación entre Artefactos

*   El **Diagrama de Arquitectura** (`architecture-v1.png`) muestra la jerarquía física y virtual.
*   El **Plan de VLANs** (`vlan-plan-v1.png`) define la segmentación L2.
*   El **Plan de IPs** (`ip-plan.es.md`) traduce esa segmentación en direcciones concretas para cada VM.
*   La **Matriz de Flujos** (`flow-matrix.es.md`) dicta qué tráfico puede cruzar entre esas IPs a través del Firewall.

## 🛠️ Herramientas de Edición

Los archivos con extensión `.drawio` han sido creados con [Draw.io], los `.md` han sido creados con [Ghostwriter], ambos en un sistema operativo Ubuntu. Para modificarlos, se recomienda abrirlos con estas herramientas (web o escritorio) y, en el caso de los archivos `.drawio`, exportar el resultado en formato `.png` de alta resolución para mantener la coherencia visual.