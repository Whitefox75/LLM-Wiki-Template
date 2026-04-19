# Karpathy LLM Wiki — Referenz

Quelle: Andrej Karpathy — [llm-wiki.md](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)
Veröffentlicht: April 2026 (5.000+ Stars bei Erstellung dieses Skill)

## Kernkonzept (paraphrasiert)

Das LLM Wiki Pattern ist eine Alternative zu RAG (Retrieval-Augmented Generation).
Statt bei jeder Query Chunks aus einem Vector-Store zu retrieven und neu zu synthetisieren,
wird Wissen **einmalig kompiliert** und dann **current gehalten**.

Das Wiki ist ein **persistentes, compoundierendes Artefakt** — nicht ein ephemeres Query-Ergebnis.

## Drei Schichten

1. `raw/` — Immutable Sources. Unveränderlich nach dem Ablegen.
2. `wiki/` — LLM-owned. Der Agent ist der primäre Autor.
3. Schema (`CLAUDE.md`) — Instruktionen für den Agent, wie das Wiki strukturiert ist.

## Drei Operations

- **Ingest:** Neue Source einarbeiten. Touchet 10–15 Pages.
- **Query:** Frage gegen das kompilierte Wiki stellen. Antwort mit Citations.
- **Lint:** Periodischer Health-Check. Findet Widersprüche, Orphans, Stubs.

## Karpathys Analogie

> "Obsidian = IDE, LLM = Programmer, Wiki = Codebase"

Der Agent ist nicht ein Generic Chatbot, der bei jeder Session neu anfängt.
Er ist ein disziplinierter Maintainer einer wachsenden Wissensbasis.

## Compounding-Effekt

- Neue Sources verbinden sich mit bestehenden Entity- und Concept-Pages.
- Gute Query-Antworten werden als neue Overview- oder Comparison-Pages gefilt.
- Lint-Rounds identifizieren Lücken, die neue Ingest-Runden triggern.
- Das Wiki wird mit jeder Operation wertvoller.

## Scale-Empfehlung (Karpathy)

Optimiert für **persönliche Wissens-Curation in einem klar abgegrenzten Domain**.
Nicht für Enterprise-RAG. Nicht für News-Feed-Modus.
Empfohlene Größe: ≤ 100–200 Sources.

## Kritik (bekannte Gegenstimmen)

Mehul Gupta und andere haben auf Scale-Limits hingewiesen:
- Token-Kosten bei Ingest steigen nicht-linear mit Page-Count.
- Contradiction-Detection ist bei großem Wiki unzuverlässig.
- Kein Ersatz für semantische Suche mit Vector-DB bei >500 Pages.

Karpathys eigene Antwort: "Es ist ein Pattern, kein Produkt."
