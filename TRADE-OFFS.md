# TRADE-OFFS.md — Scale-Limits & bekannte Schwächen

## Scale-Limits

| Limit | Schwellwert | Konsequenz / Mitigation |
|---|---|---|
| **Page-Count** | ≳ 200 Sources / ≳ 500 Pages | Index-basiertes Retrieval wird langsam. Mitigation: `qmd` installieren oder zu GraphRAG migrieren. |
| **Contradiction-Detection** | — | LLM übersieht semantisch widersprüchliche Claims in formal unterschiedlicher Formulierung. Mitigation: manuelle Lint-Reviews, `/lint` wöchentlich. |
| **Ingest-Kosten** | Token-intensiv | Jeder Ingest touchet 10–15 Pages, jeweils vollständig gelesen + geschrieben. Realer monetärer Cost bei API-basierten Workflows. |
| **Agent-Drift** | Rewrite-Schleifen | Ohne Git-Versioning riskiert man unbemerkte inhaltliche Verschiebungen über Zeit. Mitigation: `git commit` nach jedem Ingest. |
| **Entity-Disambiguation** | — | Zwei gleichnamige Entities (z.B. zwei Firmen "Apex") werden zusammengeworfen. Mitigation: Slug-Konvention mit Suffix (`Apex-Software`, `Apex-Hardware`). |
| **Nicht-textuelle Sources** | Images / Audio | Agent kann Markdown + referenzierte Images nicht in einem Pass vollständig lesen. Mitigation: erst Text ingestionieren, dann referenzierte Images separat anfordern. |
| **Kategorisierungs-Unschärfe** | Concept vs. Entity | Grenze manchmal fließend (z.B. "Transformer" = Concept oder Entity?). Mitigation: im Zweifel Entity + `aliases`-Feld nutzen. |

## Kritische Perspektiven

**Mehul Gupta** und andere haben öffentlich darauf hingewiesen, dass der Ansatz bei großem Scale (>500 Pages, >50 gleichzeitig aktive Domains) an seine Grenzen stößt — der Context-Window-Overhead bei `/ingest` wird nicht-linear teurer.

**Karpathys eigene Position:** Das ist ein **Pattern, kein Produkt**. Es skaliert nicht zu Enterprise-RAG-Systemen, ist aber für persönliche Wissens-Curation in einem klar abgegrenzten Domain ("mein Lernbereich für das nächste Jahr") sehr effektiv.

## Wann dieses System NICHT verwenden

- Mehr als 3 voneinander unabhängige Knowledge-Domains gleichzeitig
- Sources, die schneller ankommen als man ingestionieren kann (News-Feed-Modus)
- Team-Workflows mit mehreren simultanen Autoren
- Anforderung an semantische Suche ohne `qmd` und mit >200 Pages

## Migration-Pfade

| Situation | Empfehlung |
|---|---|
| > 500 Pages, Suche langsam | `qmd` installieren |
| > 1000 Pages, mehrere Domains | Zu separaten Workspace-Instanzen aufteilen |
| Team-Kontext | GraphRAG oder Notion-AI-basierter Ansatz |
| Produktions-RAG | LlamaIndex / LangChain mit Vector-DB |
