# Index-Format — wiki/index.md

## Vollständige Struktur

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

_Last updated: YYYY-MM-DD_
_Auto-maintained by Agent. Use `/reindex` to rebuild._

---

## Sources (N)

- [[sources/slug-a]] — Short description (Author, YYYY)
- [[sources/slug-b]] — Short description

---

## Entities (N)

- [[entities/EntityName]] — One-liner what this entity is
- [[entities/OtherEntity]] — One-liner

---

## Concepts (N)

- [[concepts/concept-name]] — One-liner definition
- [[concepts/other-concept]] — One-liner

---

## Comparisons (N)

- [[comparisons/a-vs-b]] — Criteria: performance, cost, ease-of-use
- [[comparisons/x-vs-y]] — Criteria: X, Y

---

## Overviews (N)

- [[overviews/domain-name]] — Scope: what this overview covers
```

## Regeln

- Sortierung: alphabetisch nach Slug/Name innerhalb jeder Sektion.
- Jeder Eintrag: `- [[type/slug]] — One-liner` (Pflicht: One-liner).
- `total_*`-Felder im Frontmatter immer mit aktuellem Count aktualisieren.
- `/reindex` baut den Index deterministisch neu auf — gleicher Wiki-Stand = gleicher Index.
- Manuelles Editieren von `index.md` ist erlaubt, aber `/reindex` überschreibt es.

## Ingest-Update vs. Reindex

- **Ingest:** Neue Einträge werden am Ende der passenden Sektion angehängt (nicht alphabetisch einsortiert — das macht `/reindex`).
- **Reindex:** Vollständiger Rebuild, alphabetisch sortiert, Counts aktualisiert.
- Empfehlung: Nach jedem größeren Ingest-Batch `/reindex` ausführen.
