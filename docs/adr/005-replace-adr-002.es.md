[Deutsch](005-replace-adr-002.md) | [English](005-replace-adr-002.en.md) | [Español](005-replace-adr-002.es.md)

# ADR 005 — Reemplazo del ADR-002 por corrección de versiones de software

- **Estado:** Aceptado
- **Hito:** M0
- **Fecha:** 2026-11-09

## Contexto

El ADR-002 (Virtualización anidada) especificaba versiones de software que no coinciden con la realidad del proyecto. En concreto, se indicaba `Ubuntu 24.04` (versión que cambio) y `Proxmox VE 8.2` (versión que cambio). Estas inconsistencias afectan a la coherencia del diseño y a la trazabilidad del proyecto. Error cometido en la documentacion, ya que el proyecto se concibio con versiones posteriores.

Según la convención de inmutabilidad de los ADRs definida en `docs/adr/README.es.md`, un ADR aceptado no se modifica. Por lo tanto, la corrección debe documentarse mediante un nuevo ADR que reemplace al anterior.

## Decisión

Se corrigen las versiones de software en el ADR-002, estableciendo:
- **Proxmox VE 9.2-1** (versión estable actual).
- **Ubuntu 26.04.1 LTS** (versión estable actual).

El **ADR-002 queda reemplazado por este ADR-005**. Se mantiene en el repositorio por trazabilidad histórica.

## Alternativas consideradas

- **Editar el ADR-002 directamente:** Viola la regla de inmutabilidad de los ADRs aceptados.
- **Dejar el ADR-002 con las versiones erróneas:** Rompe la coherencia del proyecto y confunde a los lectores.
- **Crear un nuevo ADR (esta opción):** Respeta la inmutabilidad y mantiene la trazabilidad.

## Consecuencias

**Positivas:**
- Coherencia en las versiones de software en todos los documentos.
- Trazabilidad histórica: se puede ver qué se decidió en el ADR-002 y por qué se cambió en el ADR-005.
- Respeto a la regla de inmutabilidad de los ADRs.

**Negativas:**
- Mayor número de documentos en el repositorio.
- Requiere actualizar el índice de ADRs y añadir una nota de reemplazo en el ADR-002.

## Referencias

- [`docs/adr/002-nested-virtualization.es.md`](./002-nested-virtualization.es.md) — ADR reemplazado.
- [`docs/adr/README.es.md`](./README.es.md) — Índice de ADRs actualizado.