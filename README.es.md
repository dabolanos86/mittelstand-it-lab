# Mittelstand-IT-Lab

[Deutsch](README.md) | [English](README.en.md) | [Español](README.es.md)

**Infraestructura Virtual anidada (Proxmox VE), Administración de Sistemas (Windows/Linux) y Segmentacion de Red**

> **Estado: M0 completado — Kickoff y planificación.** Este README documenta el alcance, la arquitectura objetivo y el modelo de trazabilidad. La implementación se registrará de forma incremental en el historial de commits y en `docs/`. Las evidencias de validación se guardan en `docs/validation/`.

## 1. Propósito y contexto profesional

Mittelstand-IT-Lab es un laboratorio de infraestructura personal diseñado para demostrar competencias prácticas en administración de sistemas Windows/Linux, virtualización, segmentación de red y seguridad de infraestructura IT, con un enfoque orientado a las necesidades reales de la mediana empresa alemana (Mittelstand).

El laboratorio nace de **12 años operando infraestructuras críticas** (aeropuerto internacional, banca). Por eso prioriza la **recuperación verificada**, la **segmentación de mínimo privilegio** y la **trazabilidad auditable**, no solo un entorno de pruebas funcional.

El proyecto se gestiona como un ciclo de vida formal: planificación → diseño → implementación → validación → documentación. El commit inicial constituye el acta de constitución del proyecto.

## 2. Alcance y objetivos

**Dentro del alcance**
- Host Ubuntu **26.04.1 LTS** con KVM/QEMU y libvirt como capa de virtualización.
- **Proxmox VE 9.2-1** desplegado como VM sobre dicha capa.
- **OPNsense 26.7** como firewall perimetral y motor de segmentación.
- Cargas: **Debian 13** y **Windows Server 2025**.
- Segmentación de red mediante VLANs y direccionamiento IPv4 planificado.
- Monitorización y estrategia de backup con criterios de recuperación.

**Fuera del alcance (v1)**
- Exposición a Internet de servicios productivos.
- Alta disponibilidad multi-nodo (se evaluará en v2).
- Integración con proveedores cloud.

## 3. Arquitectura objetivo

| Capa | Componente | Función |
|---|---|---|
| Host | Ubuntu 26.04.1 LTS | Sistema anfitrión, base de virtualización |
| Virtualización | KVM/QEMU + libvirt | Motor de virtualización sobre host |
| Plataforma | Proxmox VE 9.2-1 (VM) | Gestión de VMs, almacenamiento y red |
| Borde | OPNsense 26.7 | Firewall, NAT, segmentación VLAN |
| Cargas | Debian 13.7.0 / Windows Server 2025 | Servicios y pruebas de administración |
| Operación | Monitorización + Backup | Observabilidad y recuperación |

El diagrama de topología se publicará en `docs/architecture/` en el hito M2.

## 4. Roadmap y criterios de aceptación

| Hito | Entregable | Criterio de aceptación | Estado |
|---|---|---|---|
| M0 | README + alcance documentado | Commit inicial con alcance, arquitectura y modelo de trazabilidad | ✅ Completado |
| M1 | Host KVM/QEMU operativo | Anidamiento validado: `/dev/kvm` presente dentro de la VM L1, modelo de CPU `host-passthrough`, `virsh list` muestra la VM en `running`, evidencia en `docs/validation/` | 🟨 En curso |
| M2 | Proxmox VE desplegado | Proxmox accesible por web, diagrama de topología v1 publicado, configuración de la VM L1 documentada | ⬜ Pendiente |
| M3 | OPNsense + segmentación | VLANs definidas, matriz de flujos documentada, pruebas de aislamiento y permiso ejecutadas con evidencia | ⬜ Pendiente |
| M4 | Cargas Linux/Windows | Al menos un servidor Debian y uno Windows integrados, rutas de red validadas | ⬜ Pendiente |
| M5 | Monitorización y backup | Métricas visibles, restauración de backup verificada y documentada como evidencia | ⬜ Pendiente |

Cada hito se cierra con: entrada en `docs/`, actualización de esta tabla, evidencia de validación en `docs/validation/` y commit descriptivo.

## 5. Modelo de trazabilidad

- **Convención de commits:** `tipo(alcance): descripción` — p. ej. `docs(architecture): add network topology v1`.
- **Documentación por fase:** cada hito genera un documento en `docs/NN-fase.md`.
- **Registro de decisiones:** las decisiones técnicas relevantes se registran como ADR en `docs/adr/`.
- **Trazabilidad requisito → implementación → validación:** cada requisito del alcance referencia su hito y su evidencia de validación en `docs/validation/`.
- **Historial como evidencia:** el historial de commits es la fuente de verdad del progreso del proyecto.
- **ADR de proceso:** las decisiones de proceso (p. ej. estrategia de
  traducción, ver `docs/adr/001-ai-translation.es.md`) también se registran
  como ADR en `docs/adr/`, con el mismo formato que los ADR técnicos.

## 6. Gestión de riesgos (extracto)

| Riesgo | Impacto | Mitigación |
|---|---|---|
| Complejidad de virtualización anidada | Medio | Validar KVM y `/dev/kvm` dentro de la VM L1 antes de desplegar Proxmox (M1) |
| Errores de segmentación | Alto | Documentar matriz de flujos, probar conectividad por VLAN, default-deny |
| Pérdida de configuración / deriva | Alto | Backup versionado, snapshots, restauración verificada (M5) |
| Punto único de fallo (un host) | Medio | Aceptación consciente en v1, evaluación de HA en v2 |
| Deriva de alcance | Medio | Roadmap con criterios de aceptación cerrados |

## 7. Documentación y evidencias

La documentación del proyecto se mantiene en `docs/`:
- `docs/00-scope.md` — alcance y objetivos
- `docs/architecture/` — diagramas, plan de IP y decisiones de diseño
- `docs/adr/` — registros de decisiones de arquitectura y de proceso (incluye `001-ai-translation.es.md`, estrategia de traducción)
- `docs/NN-fase.md` — documentación por hito
- `docs/validation/` — evidencias de validación (capturas, salidas de comandos, resultados de pruebas)

## 8. Notas sobre idioma y autoría

- **Fuente de autoría:** español (`README.es.md`). El contenido técnico
  (arquitectura, criterios de aceptación, evidencias de validación) es
  autoría propia.
- **Versión autoritativa de publicación:** alemán (`README.md`), orientada
  al mercado objetivo (Mittelstand alemán).
- **Versión secundaria:** inglés (`README.en.md`).
- **Traducción:** DE y EN se generan con asistencia de IA y se revisan
  manualmente. La IA se emplea exclusivamente para la versión lingüística,
  no para el contenido técnico.
- **Sincronización:** ante cambios de contenido, se actualiza primero la
  fuente española y después se sincronizan DE/EN (ES → DE/EN).
- Decisión registrada en [`docs/adr/001-ai-translation.es.md`](docs/adr/001-ai-translation.es.md).


## 9.	Estado del proyecto

**Fase actual: M0 completado — Kickoff y planificación. M1 en curso.**
La infraestructura aún no está desplegada por completo. El diseño de arquitectura, la planificación de red, la implementación y las pruebas se documentarán a medida que el proyecto avance, manteniendo la trazabilidad completa desde este commit inicial. Las primeras evidencias de validación se publicarán en `docs/validation/` cuando se cierre M1.