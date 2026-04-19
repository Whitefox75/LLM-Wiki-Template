---
name: ingest
description: Einarbeiten einer neuen Source in das Wiki. Erzeugt Source-Page, updated Entity- und Concept-Pages, updated Index und Log. Der zentrale Command des LLM Wiki Patterns.
allowed-tools:
  - Read
  - Write(wiki/**)
  - Glob
  - Grep
  - Bash
---

# /ingest <path-or-url>

## WRITE-MODEL
Agent writes the wiki, human curates the raw sources.
- `raw/**` ist READ-ONLY. NIEMALS schreiben, editieren, löschen.
- `wiki/**` ist der Working-Area. Alle Outputs gehen hierhin.
- Jeder Write MUSS einen Log-Entry in `wiki/log.md` erzeugen.

## Ablauf

### Schritt 1: Source lesen
- Lokaler Pfad: Datei in `raw/` lesen.
- URL: Hinweis ausgeben, dass `/clip <url>` zuerst verwendet werden soll. Dann lokalen Pfad abwarten.
- Falls Pfad nicht in `raw/` liegt: Warnung ausgeben. Source trotzdem lesen, aber dokumentieren.

### Schritt 2: Rückfragen an User (2–3)
Stelle diese Fragen **bevor** du schreibst:
1. "Was ist der Fokus, den ich beim Ingest priorisieren soll?"
2. "Gibt es bestimmte Entities oder Concepts, die besonders wichtig sind?"
3. Falls unklar: "In welcher Sprache sollen die Wiki-Pages verfasst werden? (Sprache der Source oder andere?)"

Warte auf Antwort des Users, bevor du weitergehst.

### Schritt 3: Source-Page schreiben
Pfad: `wiki/sources/<slug>.md` (Slug: kebab-case vom Titel)
Template: `_page-templates/source.md`

Inhalt:
- **Summary:** 200–500 Wörter. Kompakte Destillation.
- **Key Claims:** Bullets. Jeder Claim verlinkt auf `[[sources/this-slug]]`.
- **Notable Quotes:** Max. 1 Quote, ≤15 Wörter.
- **Open Questions:** Unbelegte Aspekte.
- **Related Pages:** Links zu allen erzeugten/getouchten Pages.

### Schritt 4: Entity-Pages updaten/erzeugen
Für jede erwähnte Person / Firma / Produkt / Tool:
- Existierende Page: Relevante Section updaten, neue Source in `sources:`-Frontmatter hinzufügen.
- Neue Entity: Page aus `_page-templates/entity.md` anlegen.
- Bei Namenskollision: Suffix-Konvention (`Apex-Software`, `Apex-Hardware`).

### Schritt 5: Concept-Pages updaten/erzeugen
Analog zu Schritt 4 für Topics / Ideen / Methoden / Theoreme.

### Schritt 6: Contradiction-Check
Prüfe neue Claims gegen bestehende Wiki-Pages:
- Falls Widerspruch gefunden: `## Contradictions` Section in der betroffenen Page ergänzen.
- Format: `[[sources/neue-source]] behauptet X, [[sources/alte-source]] behauptet Y.`
- In der Source-Page unter `## Open Questions` vermerken.

### Schritt 7: Index updaten
`wiki/index.md`: Neue Pages in die passende Sektion einreihen.
Format: `- [[sources/slug]] — Short description`

### Schritt 8: Log-Entry appenden
```
## [YYYY-MM-DD HH:MM] ingest | <source-title>

- Pages created: [[sources/slug]], [[entities/X]], [[concepts/y]]
- Pages updated: [[entities/Z]]
- Contradictions found: [[sources/a]] ↔ [[sources/b]] on claim X
- Notes: <focus/language/special notes>
```

### Schritt 9: Diff-Report an User
Ausgabe:
- Neu erstellte Pages (mit Pfaden)
- Geupdatete Pages
- Gefundene Widersprüche
- Empfehlung: `git commit -m "ingest: <title>"`
