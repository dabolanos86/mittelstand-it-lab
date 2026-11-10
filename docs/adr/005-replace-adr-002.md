[Deutsch](005-replace-adr-002.md) | [English](005-replace-adr-002.en.md) | [Español](005-replace-adr-002.es.md)

# ADR 005 — Ersetzung des ADR-002 aufgrund einer Softwareversionskorrektur

- **Status:** Akzeptiert
- **Meilenstein:** M0
- **Datum:** 2026-11-09

## Kontext

ADR-002 (Verschachtelte Virtualisierung) gab Softwareversionen an, die nicht der Realität des Projekts entsprechen. Konkret wurden `Ubuntu 24.04` (eine geänderte Version) und `Proxmox VE 8.2` (eine geänderte Version) angegeben. Diese Inkonsistenzen beeinträchtigen die Kohärenz des Designs und die Rückverfolgbarkeit des Projekts. Es wurde ein Dokumentationsfehler gemacht, da das Projekt mit späteren Versionen konzipiert wurde.

Gemäß der Unveränderlichkeitskonvention der ADRs, die in `docs/adr/README.md` definiert ist, wird ein akzeptierter ADR nicht geändert. Daher muss die Korrektur durch einen neuen ADR dokumentiert werden, der den vorherigen ersetzt.

## Entscheidung

Die Softwareversionen in ADR-002 werden korrigiert und wie folgt festgelegt:
- **Proxmox VE 9.2-1** (aktuelle stabile Version).
- **Ubuntu 26.04.1 LTS** (aktuelle stabile Version).

**ADR-002 wird durch diesen ADR-005 ersetzt**. Er bleibt aus Gründen der historischen Rückverfolgbarkeit im Repository erhalten.

## Betrachtete Alternativen

- **ADR-002 direkt bearbeiten:** Verletzt die Unveränderlichkeitsregel akzeptierter ADRs.
- **ADR-002 mit den fehlerhaften Versionen belassen:** Bricht die Kohärenz des Projekts und verwirrt die Leser.
- **Einen neuen ADR erstellen (diese Option):** Respektiert die Unveränderlichkeit und wahrt die Rückverfolgbarkeit.

## Konsequenzen

**Positiv:**
- Kohärenz der Softwareversionen in allen Dokumenten.
- Historische Rückverfolgbarkeit: Es ist ersichtlich, was in ADR-002 entschieden wurde und warum es in ADR-005 geändert wurde.
- Einhaltung der Unveränderlichkeitsregel der ADRs.

**Negativ:**
- Größere Anzahl von Dokumenten im Repository.
- Erfordert die Aktualisierung des ADR-Index und das Hinzufügen eines Ersetzungshinweises in ADR-002.

## Referenzen

- [`docs/adr/002-nested-virtualization.md`](./002-nested-virtualization.md) — Ersetzter ADR.
- [`docs/adr/README.md`](./README.md) — Aktualisierter ADR-Index.