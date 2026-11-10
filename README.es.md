# Mittelstand-IT-Lab

[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

**Infraestructura Virtual anidada (Proxmox VE), Administración de Sistemas (Windows/Linux) y Segmentación de Red**

> **Estado: M0 completado — Kickoff y planificación.** Este README es la portada del proyecto. El detalle completo del alcance, objetivos, criterios de éxito y riesgos está en [`docs/00-scope.es.md`](docs/00-scope.es.md). La implementación se registra de forma incremental en el historial de commits y en `docs/`.

## 1. Propósito y contexto profesional

Mittelstand-IT-Lab es un laboratorio de infraestructura personal diseñado para demostrar competencias prácticas en administración de sistemas Windows/Linux, virtualización anidada, segmentación de red y seguridad de infraestructura IT, con un enfoque orientado a las necesidades reales de la mediana empresa alemana (Mittelstand).

El laboratorio nace de **12 años operando infraestructuras críticas** (aeropuerto internacional, banca). Por eso prioriza la **recuperación verificada**, la **segmentación de mínimo privilegio** y la **trazabilidad auditable**, no solo un entorno de pruebas funcional.

El alcance completo, los objetivos técnicos y de aprendizaje, los criterios de éxito, las restricciones, los supuestos y los riesgos están documentados en [`docs/00-scope.es.md`](docs/00-scope.es.md).

## 2. Arquitectura objetivo

| Capa | Componente | Función |
|---|---|---|
| Host (L0) | Ubuntu 26.04.1 LTS | Sistema anfitrión, base de virtualización |
| Virtualización | KVM/QEMU + libvirt | Motor de virtualización sobre host |
| Plataforma (L1) | Proxmox VE 9.2-1 (VM) | Gestión de VMs, almacenamiento y red |
| Borde (L2) | OPNsense 26.7 | Firewall, NAT, segmentación VLAN |
| Cargas | Debian 13 / Windows Server 2025 | Servicios y pruebas de administración |
| Operación | Monitorización + Backup | Observabilidad y recuperación |

El diagrama de arquitectura está en [`docs/architecture/architecture-v1.png`](docs/architecture/architecture-v1.png). El plan de IPs y la matriz de flujos están en [`docs/architecture/`](docs/architecture/).

## 3. Roadmap resumido

| Hito | Entregable | Estado |
|---|---|---|
| M0 | README, alcance, arquitectura y ADRs | ✅ Completado |
| M1 | Host KVM/QEMU operativo y validado | ⬜ Pendiente |
| M2 | Proxmox VE desplegado y accesible | ⬜ Pendiente |
| M3 | OPNsense + segmentación VLAN | ⬜ Pendiente |
| M4 | Cargas Linux/Windows integradas | ⬜ Pendiente |
| M5 | Monitorización y backup verificado | ⬜ Pendiente |

Los criterios de aceptación detallados de cada hito están en [`docs/00-scope.es.md`](docs/00-scope.es.md) (sección 4).

## 4. Modelo de trazabilidad

- **Convención de commits:** `tipo(alcance): descripción` — p. ej. `docs(architecture): add network topology v1`.
- **Documentación por fase:** cada hito genera un documento en `docs/NN-fase.md`.
- **Registro de decisiones:** las decisiones técnicas relevantes se registran como ADR en [`docs/adr/`](docs/adr/).
- **Trazabilidad requisito → implementación → validación:** cada requisito del alcance referencia su hito y su evidencia de validación en [`docs/validation/`](docs/validation/).
- **Historial como evidencia:** el historial de commits es la fuente de verdad del progreso del proyecto.

## 5. Documentación y evidencias

La documentación del proyecto se mantiene en `docs/`:
- [`docs/00-scope.es.md`](docs/00-scope.es.md) — Alcance, objetivos, criterios de éxito, riesgos.
- [`docs/architecture/`](docs/architecture/) — Diagramas, plan de IPs y matriz de flujos.
- [`docs/adr/`](docs/adr/) — Registro de decisiones de arquitectura y de proceso.
- [`docs/validation/`](docs/validation/) — Evidencias de validación (capturas, salidas de comandos, resultados de pruebas).

## 6. Notas sobre idioma y autoría

- **Fuente de autoría:** español (`README.es.md`). El contenido técnico (arquitectura, criterios de aceptación, evidencias de validación) es autoría propia.
- **Versión autoritativa de publicación:** alemán (`README.md`), orientada al mercado objetivo (Mittelstand alemán).
- **Versión secundaria:** inglés (`README.en.md`).
- **Traducción:** DE y EN se generan con asistencia de IA y se revisan manualmente. La IA se emplea exclusivamente para la versión lingüística, no para el contenido técnico.
- **Sincronización:** ante cambios de contenido, se actualiza primero la fuente española y después se sincronizan DE/EN (ES → DE/EN).
- Decisión registrada en [`docs/adr/001-ai-translation.es.md`](docs/adr/001-ai-translation.es.md).

## 7. Estado del proyecto

**Fase actual: M0 completado — Kickoff y planificación. M1 en curso.**
La infraestructura aún no está desplegada por completo. El diseño de arquitectura, la planificación de red, la implementación y las pruebas se documentarán a medida que el proyecto avance, manteniendo la trazabilidad completa desde este commit inicial. Las primeras evidencias de validación se publicarán en [`docs/validation/`](docs/validation/) cuando se cierre M1.