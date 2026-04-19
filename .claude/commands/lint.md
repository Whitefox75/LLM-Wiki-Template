---
name: lint
description: Periodischer Health-Check des gesamten Wiki. Findet Contradictions, Orphans, Stubs, Missing Concepts, Stale Claims, Broken Links, Index-Drift. Keine automatischen Fixes.
allowed-tools:
  - Read
  - Write(wiki/lint-reports/**)
  - Write(wiki/log.md)
  - Glob
  - Grep
  - Bash
---

# /lint

## WRITE-MODEL
Agent writes the wiki, human curates the raw sources.
- `/lint` schreibt NUR in `wiki/lint-reports/` und `wiki/log.md`.
- Keine automatischen Fixes in anderen Wiki-Pages.
- User entscheidet pro Item, was zu tun ist.

## Ablauf

Scanne `wiki/` vollständig. Erzeuge Report in `wiki/lint-reports/YYYY-MM-DD.md`.

### Check 1: Contradictions
```bash
grep -r "## Contradictions" wiki/ --include="*.md" -l
```
Für jede gefundene Page: Section-Inhalt lesen und im Report dokumentieren.

### Check 2: Orphans
Für jede Page in `wiki/` (außer `index.md` und `log.md`):
```bash
grep -r "[[PageName]]" wiki/ --include="*.md" | grep -v "^wiki/index.md"
```
Pages ohne inbound Links → Orphan.

### Check 3: Stubs
Für jede Page: Body-Text-Wörter zählen (ohne Frontmatter, ohne Headings).
Pages < 50 Wörter → Stub.

### Check 4: Missing Concepts
Extrahiere alle Terme, die in ≥3 Pages vorkommen, aber keine eigene `wiki/concepts/`-Page haben.
```bash
# Pseudo-Logik: grep häufiger Terme, abgleichen gegen wiki/concepts/
```

### Check 5: Stale Claims
Pages ohne `evergreen: true` in Frontmatter, deren verlinkte Sources älter als 180 Tage sind.
Datum aus `ingested_at` Frontmatter der Source-Pages auslesen.

### Check 6: Broken Links
Alle `[[...]]`-Links in `wiki/` extrahieren. Prüfen ob Ziel-File existiert.
```bash
grep -ro "\[\[.*\]\]" wiki/ | sed 's/.*\[\[//;s/\]\].*//'
```

### Check 7: Index-Drift
- Pages in `wiki/` aber nicht in `wiki/index.md` → fehlende Einträge.
- Einträge in `wiki/index.md` aber kein entsprechendes File → tote Links.

## Report-Format

```markdown
# Lint Report — YYYY-MM-DD

## Summary
- Contradictions: N
- Orphans: N
- Stubs: N
- Missing Concepts: N
- Stale Claims: N
- Broken Links: N
- Index-Drift: N

## Contradictions
- [[page]] ↔ [[page2]] — Claim X

## Orphans
- [[entities/X]] — keine inbound links

## Stubs
- [[concepts/y]] — 23 Wörter

## Missing Concepts
- "Begriff" — erwähnt in [[p1]], [[p2]], [[p3]]

## Stale Claims
- [[sources/old-source]] — ingested 2025-06-01, kein evergreen

## Broken Links
- [[entities/ghost]] — referenziert in [[sources/x]], File existiert nicht

## Index-Drift
- In wiki/, nicht im Index: [[concepts/z]]
- Im Index, kein File: [[entities/deleted]]

## Recommended Actions
- Fix Contradiction in [[page]]: re-ingest [[sources/a]] mit neuem Fokus
- Delete Orphan [[entities/X]] oder verlinken
- Expand Stub [[concepts/y]]
- Create Concept-Page für "Begriff"
```

## Log-Entry

```
## [YYYY-MM-DD HH:MM] lint | <N> issues found

- Report: [[lint-reports/YYYY-MM-DD]]
- Critical: <Contradictions-Count> contradictions, <Broken-Links-Count> broken links
```
