[Deutsch](001-ai-translation.md) | [English](001-ai-translation.en.md) | [Español](001-ai-translation.es.md)
# ADR 001 — Estrategia de traducción y versiones de idioma del README

- **Estado:** Aceptado
- **Hito:** M0
- **Fecha:** 2026-11-01

## Contexto

El autor es hispanohablante nativo y escribe el contenido técnico
principalmente en español. El mercado objetivo del proyecto es la mediana
empresa alemana (Hannover). El repositorio se publica en tres idiomas
(DE/EN/ES). Una traducción profesional de todos los documentos sería
desproporcionadamente costosa en el arranque del proyecto.

## Decisión

- **Fuente de contenido (autoría):** español (`README.es.md`).
- **Versión principal de publicación:** alemán (`README.md`) — es la primera
  versión que ven los reclutadores en el repositorio y la autoritativa para
  el mercado laboral alemán.
- **Versión secundaria:** inglés (`README.en.md`).
- **Proceso de traducción:** DE y EN se generan con asistencia de IA y se
  revisan manualmente. El contenido técnico (arquitectura, criterios de
  aceptación, evidencias de validación) es autoría propia; la IA se emplea
  exclusivamente para la versión lingüística.
- La estrategia se declara en el README (sección 8 «Notas sobre idioma y autoría»).

## Alternativas

- **Solo alemán:** reduce el alcance y dificulta el mantenimiento por parte del autor.
- **Solo español:** no apto para el mercado objetivo.
- **Traducción profesional:** demasiado costosa para un laboratorio personal.
- **No declararlo:** contradice el modelo de trazabilidad del proyecto.

## Consecuencias

**Positivas:**
- Separación clara entre autoría (ES) y publicación (DE).
- Transparencia sobre el método, coherente con el modelo de trazabilidad.
- Esfuerzo reducido con calidad aceptable.

**Negativas:**
- Se requiere revisión manual para evitar calcos y errores terminológicos.
- Ante cambios de contenido, primero se actualiza la fuente española y
  después se sincronizan DE/EN (ES → DE/EN).
- La versión alemana debe mantenerse terminológicamente consistente
  (Meilenstein, Abnahmekriterium, Liefergegenstand, Nachweis).