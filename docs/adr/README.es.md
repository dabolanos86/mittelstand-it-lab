[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)
# Architecture Decision Records (ADRs)

**Documento:** Índice de Decisiones de Arquitectura  
**Versión:** 1.0  
**Hito:** M0 - Kickoff & Planning  
**Estado:** Propuesto  
**Fecha:** 2026-11-10 

## 📌 ¿Qué es un ADR?

Un **Architecture Decision Record (ADR)** es un documento breve que captura una decisión de arquitectura importante y de proceso, junto con su contexto y sus consecuencias. Su objetivo es proporcionar **trazabilidad** sobre *por qué* se tomó una decisión, no solo *qué* se decidió.

En este proyecto, los ADRs siguen el formato estándar de Michael Nygard:
- **Contexto:** La situación que motiva la decisión.
- **Decisión:** La opción elegida.
- **Alternativas:** Las opciones consideradas y descartadas.
- **Consecuencias:** Los efectos positivos y negativos de la decisión.

## 📋 Convenciones

- **Numeración secuencial:** `NNN-titulo-descriptivo.idioma.md` (ej. `001-ai-translation.es.md`).
- **Inmutabilidad:** Un ADR aceptado no se modifica. Si una decisión cambia, se crea un nuevo ADR que reemplaza al anterior (ej. `005-replace-adr-002.md`).
- **Idiomas:** Cada ADR se publica en español (fuente), inglés y alemán, con enlaces cruzados en la cabecera.
- **Estado:** `Propuesto`, `Aceptado`, `Rechazado`, `Reemplazado por NNN`.

## 🗂️ Índice de ADRs

| ID | Título | Hito | Estado | Idiomas |
| :--- | :--- | :--- | :--- | :--- |
| [001](./001-ai-translation.es.md) | Estrategia de traducción y versiones de idioma del README | M0 | ✅ Aceptado | [ES](./001-ai-translation.es.md) · [EN](./001-ai-translation.en.md) · [DE](./001-ai-translation.md) |
| [002](./002-nested-virtualization.es.md) | Virtualización anidada: Proxmox VE sobre KVM/QEMU | M0 |⚠️ Reemplazado por [ADR-005](./005-replace-adr-002.es.md) | [ES](./002-nested-virtualization.es.md) · [EN](./002-nested-virtualization.en.md) · [DE](./002-nested-virtualization.md) |
| [003](./003-opnsense-vs-pfsense.es.md) | Elección de OPNsense sobre pfSense como firewall perimetral | M0 | ✅ Aceptado | [ES](./003-opnsense-vs-pfsense.es.md) · [EN](./003-opnsense-vs-pfsense.en.md) · [DE](./003-opnsense-vs-pfsense.md) |
| [004](./004-vlan-design.es.md) | Diseño de VLANs y direccionamiento VLSM | M0 | ✅ Aceptado | [ES](./004-vlan-design.es.md) · [EN](./004-vlan-design.en.md) · [DE](./004-vlan-design.md) |
| [005](./005-replace-adr-002.es.md) | Reemplazo del ADR-002 por corrección de versiones de software | M0 | ✅ Aceptado | [ES](./005-replace-adr-002.es.md) · [EN](./005-replace-adr-002.en.md) · [DE](./005-replace-adr-002.md) |

> **Nota:** La tabla se actualizará a medida que se añadan nuevos ADRs en hitos posteriores (M1, M2, M3...).

## 🔗 Relación con otros documentos

- **`docs/architecture/`**: Contiene los diagramas y planes que implementan las decisiones aquí registradas.
- **`docs/validation/`**: Contiene las evidencias que demuestran que las decisiones funcionan en la práctica.
- **`README.md` (raíz)**: La sección 5 ("Modelo de trazabilidad") explica cómo los ADRs encajan en el ciclo de vida del proyecto.