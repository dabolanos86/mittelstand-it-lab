[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

# Documentación del Proyecto — Mittelstand-IT-Lab

**Documento:** Índice General de Documentación  
**Versión:** 1.0  
**Hito:** M0 - Kickoff & Planning  
**Estado:** Propuesto  
**Fecha:** 2026-11-10  

## 📌 Propósito de este directorio

Esta carpeta contiene la documentación técnica del proyecto, organizada por hitos y por tipo de artefacto. Aquí se registra el ciclo de vida completo: planificación (M0), implementación (M1-M2), segmentación (M3), cargas (M4) y operación (M5).

## 🗂️ Índice General

### Documentos Transversales

| Documento | Descripción | Idiomas |
| :--- | :--- | :--- |
| [`00-scope.es.md`](./00-scope.es.md) | Alcance, objetivos, criterios de éxito y riesgos del proyecto. | [ES](./00-scope.es.md) · [EN](./00-scope.en.md) · [DE](./00-scope.md) |

### Arquitectura y Red

| Documento | Descripción | Idiomas |
| :--- | :--- | :--- |
| [`architecture/`](./architecture/) | Diagramas, plan de IPs y matriz de flujos. | [ES](./architecture/README.es.md) · [EN](./architecture/README.en.md) · [DE](./architecture/README.md) |

### Decisiones de Arquitectura (ADRs)

| Documento | Descripción | Idiomas |
| :--- | :--- | :--- |
| [`adr/`](./adr/) | Registro de decisiones técnicas y de proceso. | [ES](./adr/README.es.md) · [EN](./adr/README.en.md) · [DE](./adr/README.md) |

### Documentación por Hito

| Hito | Documento | Estado |
| :--- | :--- | :--- |
| M0 | `00-scope.es.md` | ✅ Completado |
| M1 | `01-host-kvm-audit.md` | ⬜ Pendiente |
| M2 | `02-proxmox-deployment.md` | ⬜ Pendiente |
| M3 | `03-opnsense-segmentation.md` | ⬜ Pendiente |
| M4 | `04-workloads-deployment.md` | ⬜ Pendiente |
| M5 | `05-monitoring-backup.md` | ⬜ Pendiente |

### Evidencias de Validación

| Carpeta | Descripción |
| :--- | :--- |
| [`validation/`](./validation/) | Capturas, salidas de comandos y resultados de pruebas. |

## 🔗 Relación con otros documentos

- **`README.es.md` (raíz)**: Documento principal del proyecto (acta de constitución).
- **`docs/architecture/`**: Diseño lógico y de red.
- **`docs/adr/`**: Decisiones de arquitectura y proceso.
- **`docs/validation/`**: Evidencias de que el sistema funciona como se diseñó.

## 🛠️ Convenciones

- **Numeración por hito:** `NN-titulo-descriptivo.md` (ej. `01-host-kvm-audit.md`).
- **Idiomas:** Cada documento se publica en español (fuente), inglés y alemán.
- **Estado:** `Propuesto`, `Aceptado`, `En curso`, `Completado`.