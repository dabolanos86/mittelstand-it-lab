[Deutsch](001-ai-translation.md) | [English](001-ai-translation.en.md) | [Español](001-ai-translation.es.md)

# ADR 001 — Übersetzungsstrategie und Sprachversionen der README

- **Status:** Akzeptiert
- **Meilenstein:** M0
- **Datum:** 2026-11-01

## Kontext

Der Autor ist spanischer Muttersprachler und verfasst den technischen Inhalt
überwiegend auf Spanisch. Der Zielmarkt des Projekts ist der deutsche
Mittelstand (Hannover). Das Repository wird in drei Sprachen
(DE/EN/ES) veröffentlicht. Eine professionelle Übersetzung aller Dokumente
wäre zu Projektbeginn unverhältnismäßig teuer.

## Entscheidung

- **Inhaltsquelle (Autorschaft):** Spanisch (`README.es.md`).
- **Primäre Veröffentlichungsversion:** Deutsch (`README.md`) — sie ist die erste
  Version, die Personalvermittler im Repository sehen, und die maßgebliche für
  den deutschen Arbeitsmarkt.
- **Sekundärversion:** Englisch (`README.en.md`).
- **Übersetzungsprozess:** DE und EN werden mit KI-Unterstützung erstellt und
  manuell überprüft. Der technische Inhalt (Architektur, Abnahmekriterien,
  Validierungsnachweise) ist Eigenarbeit; KI wird ausschließlich für die
  sprachliche Version eingesetzt.
- Die Strategie wird in der README erklärt (Abschnitt 8 „Hinweise zu Sprache und Autorschaft").

## Alternativen

- **Nur Deutsch:** reduziert den Umfang und erschwert die Wartung durch den Autor.
- **Nur Spanisch:** nicht geeignet für den Zielmarkt.
- **Professionelle Übersetzung:** zu teuer für ein persönliches Labor.
- **Nicht deklarieren:** widerspricht dem Traceability-Modell des Projekts.

## Konsequenzen

**Positiv:**
- Klare Trennung zwischen Autorschaft (ES) und Veröffentlichung (DE).
- Transparenz über die Methode, konsistent mit dem Traceability-Modell.
- Geringer Aufwand bei akzeptabler Qualität.

**Negativ:**
- Manuelle Überprüfung erforderlich, um Lehnübersetzungen und terminologische Fehler zu vermeiden.
- Bei Inhaltsänderungen wird zuerst die spanische Quelle aktualisiert und
  anschließend DE/EN synchronisiert (ES → DE/EN).
- Die deutsche Version muss terminologisch konsistent bleiben
  (Meilenstein, Abnahmekriterium, Liefergegenstand, Nachweis).