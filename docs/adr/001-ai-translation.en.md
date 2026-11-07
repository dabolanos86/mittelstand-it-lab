[Deutsch](001-ai-translation.md) | [English](001-ai-translation.en.md) | [Español](001-ai-translation.es.md)

# ADR 001 — Translation Strategy and README Language Versions

- **Status:** Accepted
- **Milestone:** M0
- **Date:** 2026-11-01

## Context

The author is a native Spanish speaker and writes the technical content
primarily in Spanish. The project's target market is the German mid-sized
enterprise (Hannover). The repository is published in three languages
(DE/EN/ES). A professional translation of all documents would be
disproportionately expensive at the project's inception.

## Decision

- **Content source (authorship):** Spanish (`README.es.md`).
- **Primary publication version:** German (`README.md`) — it is the first
  version recruiters see in the repository and the authoritative one for
  the German job market.
- **Secondary version:** English (`README.en.md`).
- **Translation process:** DE and EN are generated with AI assistance and
  manually reviewed. The technical content (architecture, acceptance
  criteria, validation evidence) is original work; AI is used exclusively
  for the linguistic version.
- The strategy is declared in the README (section 8 "Notes on language and authorship").

## Alternatives

- **German only:** reduces scope and makes maintenance harder for the author.
- **Spanish only:** unsuitable for the target market.
- **Professional translation:** too expensive for a personal lab.
- **Not declaring it:** contradicts the project's traceability model.

## Consequences

**Positive:**
- Clear separation between authorship (ES) and publication (DE).
- Transparency about the method, consistent with the traceability model.
- Reduced effort with acceptable quality.

**Negative:**
- Manual review is required to avoid calques and terminological errors.
- On content changes, the Spanish source is updated first and
  then DE/EN are synchronized (ES → DE/EN).
- The German version must remain terminologically consistent
  (Meilenstein, Abnahmekriterium, Liefergegenstand, Nachweis).