---
name: reindex
description: Rebuild von wiki/index.md from scratch durch Scan aller Files in wiki/. Vorher Backup. Deterministisch — gleicher Wiki-Stand erzeugt gleichen Index.
allowed-tools:
  - Read
  - Write(wiki/index.md)
  - Write(wiki/index.md.bak)
  - Write(wiki/log.md)
  - Glob
  - Bash
---

# /reindex

## WRITE-MODEL
Agent writes the wiki, human curates the raw sources.
- Schreibt NUR in `wiki/index.md`, `wiki/index.md.bak`, `wiki/log.md`.
- `raw/**` bleibt unangetastet.

## Ablauf

### Schritt 1: Backup
```bash
cp wiki/index.md wiki/index.md.bak
```
Falls `wiki/index.md` nicht existiert: überspringen.

### Schritt 2: Wiki-Files scannen
```bash
find wiki/ -name "*.md" -not -name "index.md" -not -name "log.md" -not -path "wiki/lint-reports/*"
```

Für jede gefundene File:
- Frontmatter lesen (type, title/name, domain)
- Kurzbeschreibung extrahieren (erste Nicht-Leer-Zeile nach dem ersten H2)

### Schritt 3: Index aufbauen

Struktur:
```markdown
---
type: index
last_updated: "YYYY-MM-DD"
total_sources: N
total_entities: N
total_concepts: N
total_comparisons: N
total_overviews: N
---

# Wiki Index

_Last updated: YYYY-MM-DD — rebuilt via /reindex_
_Auto-maintained by Agent. Use `/reindex` to rebuild._

---

## Sources (N)
- [[sources/slug]] — Short description

## Entities (N)
- [[entities/Name]] — One-liner

## Concepts (N)
- [[concepts/name]] — One-liner

## Comparisons (N)
- [[comparisons/a-vs-b]] — Criteria: X, Y

## Overviews (N)
- [[overviews/domain]] — Scope
```

Sortierung innerhalb jeder Sektion: alphabetisch nach Slug/Name.

### Schritt 4: Index schreiben
`wiki/index.md` komplett überschreiben mit dem neu aufgebauten Index.

### Schritt 5: Log-Entry
```
## [YYYY-MM-DD HH:MM] reindex | Full rebuild

- Sources: N
- Entities: N
- Concepts: N
- Comparisons: N
- Overviews: N
- Backup: wiki/index.md.bak
```

### Schritt 6: Report an User
- Counts pro Kategorie
- Hinweis: Backup liegt unter `wiki/index.md.bak`
- Falls Abweichungen zum alten Index festgestellt: auflisten (neue Pages, fehlende Einträge)
