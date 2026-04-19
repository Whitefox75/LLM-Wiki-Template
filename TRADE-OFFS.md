# TRADE-OFFS.md — Scale Limits & Known Weaknesses

> Full reference: [README.md](./README.md) | Setup: [SETUP.md](./SETUP.md) | Workflow: [WORKFLOW.md](./WORKFLOW.md)

## LLM Wiki vs. traditional RAG

| Dimension | Traditional RAG | LLM Wiki |
|---|---|---|
| **Knowledge state** | Stateless — rebuilt every time | Stateful — grows with every ingestion |
| **Contradictions** | Silent | Explicitly detected and flagged |
| **Citations** | Approximate ("chunk 3") | Exact (`[[sources/slug]]`) |
| **Human control** | Opaque retrieval pipeline | Human-in-the-loop at every write operation |
| **Scale** | Unlimited (vector DB) | ≤ 200 sources / ≤ 500 pages |
| **Setup effort** | High (embedding, vector DB, pipeline) | Low (folder + agent) |

## Scale Limits

| Limit | Threshold | Consequence / Mitigation |
|---|---|---|
| **Page count** | > 200 sources / > 500 pages | Index-based retrieval slows down. Mitigation: install `qmd` or migrate to GraphRAG. |
| **Contradiction detection** | — | The LLM misses semantically contradictory claims phrased differently. Mitigation: manual lint reviews, run `/lint` regularly. |
| **Ingest cost** | Token-intensive | Every ingest touches 10–15 pages, each read and written in full. Real monetary cost in API-based workflows. |
| **Agent drift** | Rewrite loops | Without Git versioning, silent content drift accumulates over time. Mitigation: `git commit` after every ingest. |
| **Entity disambiguation** | — | Two same-named entities (e.g. two companies named "Apex") get merged. Mitigation: slug suffix convention (`Apex-Software`, `Apex-Hardware`). |
| **Non-textual sources** | Images / audio | Agent cannot fully read Markdown + referenced images in a single pass. Mitigation: ingest text first, then request referenced images separately. |
| **Classification ambiguity** | Concept vs. entity | The boundary is sometimes blurry (e.g. "Transformer" = concept or entity?). Mitigation: when in doubt, use Entity + `aliases` field. |

## Critical perspectives

**Mehul Gupta** and others have publicly noted that the approach hits its limits at large scale (>500 pages, >50 simultaneously active domains) — the context-window overhead for `/ingest` grows non-linearly.

**Karpathy's own position:** This is a **pattern, not a product**. It does not scale to enterprise RAG systems, but is highly effective for personal knowledge curation within a clearly bounded domain ("my learning focus for the next year").

## When NOT to use this system

- More than 3 mutually independent knowledge domains simultaneously
- Sources arriving faster than you can ingest them (news-feed mode)
- Team workflows with multiple simultaneous authors
- Requirement for semantic search without `qmd` and with >200 pages
- When source data is primarily non-textual (audio, video, images)

## Migration paths

| Situation | Recommendation |
|---|---|
| > 200 sources, search is slow | Install `qmd` (`pip install qmd && qmd index wiki/`) |
| > 500 pages, multiple domains | Separate workspace instances per domain |
| > 1000 pages | GraphRAG or LlamaIndex with vector DB |
| Team context | GraphRAG or Notion-AI-based approach |
| Production RAG | LlamaIndex / LangChain with vector DB |

---

---

# TRADE-OFFS.md — Scale-Limits & bekannte Schwächen *(Deutsch)*

> Vollständige Referenz: [README.md](./README.md) | Setup: [SETUP.md](./SETUP.md) | Workflow: [WORKFLOW.md](./WORKFLOW.md)

## LLM Wiki vs. klassisches RAG

| Dimension | Klassisches RAG | LLM Wiki |
|---|---|---|
| **Wissenszustand** | Stateless — jedes Mal neu aufgebaut | Stateful — wächst mit jeder Ingestion |
| **Widersprüche** | Stumm | Explizit erkannt und markiert |
| **Citations** | Approximativ ("Chunk 3") | Exakt (`[[sources/slug]]`) |
| **Human Control** | Opaker Retrieval-Pipeline | Human-in-the-loop bei jeder Schreiboperation |
| **Scale** | Beliebig (Vector-DB) | ≤ 200 Sources / ≤ 500 Pages |
| **Setup-Aufwand** | Hoch (Embedding, Vector-DB, Pipeline) | Gering (Ordner + Agent) |

## Scale-Limits

| Limit | Schwellwert | Konsequenz / Mitigation |
|---|---|---|
| **Page-Count** | > 200 Sources / > 500 Pages | Index-basiertes Retrieval wird langsam. Mitigation: `qmd` installieren oder zu GraphRAG migrieren. |
| **Contradiction-Detection** | — | LLM übersieht semantisch widersprüchliche Claims in formal unterschiedlicher Formulierung. Mitigation: manuelle Lint-Reviews, `/lint` regelmäßig. |
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
- Wenn die Quelldaten primär nicht-textuell sind (Audio, Video, Bilder)

## Migration-Pfade

| Situation | Empfehlung |
|---|---|
| > 200 Sources, Suche langsam | `qmd` installieren (`pip install qmd && qmd index wiki/`) |
| > 500 Pages, mehrere Domains | Separate Workspace-Instanzen pro Domain |
| > 1000 Pages | GraphRAG oder LlamaIndex mit Vector-DB |
| Team-Kontext | GraphRAG oder Notion-AI-basierter Ansatz |
| Produktions-RAG | LlamaIndex / LangChain mit Vector-DB |
