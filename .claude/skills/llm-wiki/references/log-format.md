# Log-Format — wiki/log.md

## Vollständige Struktur

```markdown
---
type: log
format: "## [YYYY-MM-DD HH:MM] <operation> | <short-title>"
---

# Wiki Log

_Chronological, append-only. Do not reorder or edit past entries._
_Grep all entries: `grep "^## \[" wiki/log.md`_

---

## [2026-04-19 14:30] ingest | Karpathy LLM Wiki Gist

- Pages created: [[sources/karpathy-llm-wiki]], [[entities/AndrejKarpathy]], [[concepts/compile-once-maintain-forever]]
- Pages updated: [[wiki/index]]
- Contradictions found: none
- Notes: First ingest. Source language: English. Focus: core pattern.

---

## [2026-04-19 15:00] query | Unterschied Wiki vs RAG

- Pages consulted: [[concepts/compile-once-maintain-forever]], [[sources/karpathy-llm-wiki]]
- Filed as: none (User declined)

---

## [2026-04-19 16:00] lint | 3 issues found

- Report: [[lint-reports/2026-04-19]]
- Orphans: [[entities/SomeName]]
- Stubs: [[concepts/foo]] (32 words)
- Missing Concepts: "attention mechanism"

---

## [2026-04-20 09:00] file | compile-once-overview

- Filed as: [[overviews/compile-once-overview]]
- Type: overview
- Source query: "Was ist der Unterschied zwischen LLM-Wiki und RAG?"
- Citations preserved: [[concepts/compile-once-maintain-forever]], [[sources/karpathy-llm-wiki]]

---

## [2026-04-21 10:00] reindex | Full rebuild

- Sources: 3
- Entities: 5
- Concepts: 8
- Comparisons: 1
- Overviews: 2
- Backup: wiki/index.md.bak

---

## [2026-04-21 11:00] schema-update | Added evergreen field to concept template

- Changed: _page-templates/concept.md — added evergreen: false
- Changed: CLAUDE.md — documented evergreen behavior in Lint section
```

## Regeln

| Regel | Detail |
|---|---|
| **Append-only** | Niemals bestehende Entries editieren oder löschen |
| **H2-Format** | `## [YYYY-MM-DD HH:MM] <op> | <title>` — exakt so |
| **Grep-kompatibel** | `grep "^## \[" wiki/log.md` liefert saubere Zeilenliste |
| **Pflicht-Fields** | Jeder Entry hat mindestens einen Bullet mit Kontext |
| **Separator** | `---` zwischen Entries (optional, aber empfohlen für Lesbarkeit) |

## Valide Operations-Labels

- `bootstrap` — Workspace-Initialisierung
- `ingest` — Source eingearbeitet
- `query` — Frage gestellt
- `file` — Antwort als Page gespeichert
- `lint` — Health-Check
- `reindex` — Index neu aufgebaut
- `clip` — URL geclipt
- `schema-update` — CLAUDE.md oder Templates geändert
- `manual-edit` — User hat direkt in Wiki-Page eingegriffen
