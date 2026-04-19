# LLM Wiki Template

> A Karpathy-style persistent knowledge artifact — compiled once, kept current, machine-readable by design.
>
> Inspired by [Andrej Karpathy's llm-wiki.md](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) (April 2026)

---

## Why not RAG?

| | Traditional RAG | LLM Wiki |
|---|---|---|
| **Per query** | Chunk → embed → retrieve → reconstruct | Read compiled wiki → answer with citations |
| **Knowledge state** | Stateless — rebuilt every time | Stateful — grows with every ingestion |
| **Contradictions** | Silent | Detected and flagged explicitly |
| **Citations** | Approximate ("based on chunk 3") | Exact (`[[sources/slug]]` wikilinks) |
| **Human control** | Opaque retrieval pipeline | Human-in-the-loop at every write operation |

The core idea: **treat your knowledge base like a codebase.** The agent is the programmer, the wiki is the compiled output, raw sources are immutable inputs.

```
Obsidian = IDE
You       = Architect
Agent     = Programmer
Wiki      = Codebase
```

---

## How it works

```
raw/              ← Your immutable source documents (PDFs, notes, clipped URLs)
wiki/             ← Agent-maintained knowledge base (Sources, Entities, Concepts, ...)
_page-templates/  ← Frontmatter skeletons per page type
tools/            ← Utility scripts
.claude/          ← Claude Code commands & agent schema (CLAUDE.md)
```

**One ingestion → 10–15 wiki pages touched.** Each source fans out into Entity pages (people, tools, companies), Concept pages (topics, methods), and a Source summary — all cross-linked.

### Operations

| Command | What it does | When |
|---|---|---|
| `/bootstrap` | Generates the full folder structure | Once, on setup |
| `/ingest <path\|url>` | Processes a source into the wiki | Ad-hoc |
| `/clip <url>` | Clips a URL to `raw/` before ingestion | Ad-hoc |
| `/query <question>` | Synthesizes answers from wiki with citations | Ad-hoc |
| `/file <slug>` | Saves a query result as a permanent wiki page | Weekly |
| `/lint` | Reports contradictions, orphans, stubs, broken links | Monthly |
| `/search <term>` | Full-text search across the wiki | Ad-hoc |
| `/reindex` | Rebuilds `wiki/index.md` | After bulk changes |

### Human-in-the-loop by design

The agent **never** writes to `raw/`, never auto-fixes lint issues, and never files query results without explicit instruction. Every write decision is yours.

---

## Real-world example

This template was built alongside a practical use case:

- **Input:** A personal Obsidian vault for IT vocational training (networking, OS, security, project management, IT law)
- **Output:** A structured LLM Wiki — machine-readable, citation-linked, contradiction-aware
- **Result:** The same knowledge base serves both human review (Obsidian graph) and agent queries (structured wiki traversal) without duplication

The key insight: **your human-curated notes become structured agent input.** The wiki is not a copy of your notes — it is a compiled, normalized, cross-linked representation optimized for LLM consumption.

---

## Quickstart

### Requirements

- [Obsidian](https://obsidian.md/) v1.12+
- [Claude Code](https://claude.ai/claude-code) (current version)
- Git (recommended)
- Optional: `qmd` for full-text search performance at scale (>200 sources)

### Setup

```bash
# 1. Clone the template
git clone https://github.com/Whitefox75/LLM-Wiki-Template.git my-wiki
cd my-wiki

# 2. Open the folder in Obsidian as a new vault

# 3. In Claude Code, run:
/bootstrap
```

Then drop a source file into `raw/` and run `/ingest <filename>`. Watch the wiki grow.

Full setup guide: [SETUP.md](./SETUP.md) | Workflow reference: [WORKFLOW.md](./WORKFLOW.md) | Scale limits: [TRADE-OFFS.md](./TRADE-OFFS.md)

---

## Scale

Optimized for **≤ 200 sources / ≤ 500 pages**. Beyond that, index-based retrieval slows down — see [TRADE-OFFS.md](./TRADE-OFFS.md) for migration paths (qmd, GraphRAG).

---

## License

MIT

---

---

# LLM Wiki Template *(Deutsch)*

> Ein Karpathy-Style persistentes Wissensartefakt — einmal kompiliert, aktuell gehalten, maschinell lesbar by design.
>
> Inspiriert von [Andrej Karpathys llm-wiki.md](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) (April 2026)

---

## Warum kein RAG?

| | Klassisches RAG | LLM Wiki |
|---|---|---|
| **Pro Query** | Chunk → embed → retrieve → rekonstruieren | Kompiliertes Wiki lesen → Antwort mit Citations |
| **Wissenszustand** | Stateless — jedes Mal neu aufgebaut | Stateful — wächst mit jeder Ingestion |
| **Widersprüche** | Stumm | Explizit erkannt und markiert |
| **Citations** | Approximativ ("basierend auf Chunk 3") | Exakt (`[[sources/slug]]` Wikilinks) |
| **Human Control** | Opaker Retrieval-Pipeline | Human-in-the-loop bei jeder Schreiboperation |

Die Kernidee: **Behandle deine Wissensbasis wie eine Codebase.** Der Agent ist der Programmer, das Wiki ist der kompilierte Output, Raw Sources sind immutable Inputs.

```
Obsidian = IDE
Du        = Architekt
Agent     = Programmer
Wiki      = Codebase
```

---

## Wie es funktioniert

```
raw/              ← Deine immutablen Quelldokumente (PDFs, Notizen, geclippte URLs)
wiki/             ← Agenten-gepflegte Wissensbasis (Sources, Entities, Concepts, ...)
_page-templates/  ← Frontmatter-Skeletons pro Page-Typ
tools/            ← Hilfsskripte
.claude/          ← Claude Code Commands & Agenten-Schema (CLAUDE.md)
```

**Eine Ingestion → 10–15 Wiki-Pages berührt.** Jede Source verzweigt in Entity-Pages (Personen, Tools, Firmen), Concept-Pages (Themen, Methoden) und eine Source-Summary — alles cross-linked.

### Operationen

| Befehl | Funktion | Wann |
|---|---|---|
| `/bootstrap` | Erzeugt die vollständige Ordnerstruktur | Einmalig beim Setup |
| `/ingest <path\|url>` | Verarbeitet eine Source ins Wiki | Ad-hoc |
| `/clip <url>` | Clippt eine URL nach `raw/` vor der Ingestion | Ad-hoc |
| `/query <frage>` | Synthetisiert Antworten aus dem Wiki mit Citations | Ad-hoc |
| `/file <slug>` | Speichert ein Query-Ergebnis als permanente Wiki-Page | Wöchentlich |
| `/lint` | Meldet Widersprüche, Orphans, Stubs, kaputte Links | Monatlich |
| `/search <term>` | Volltextsuche über das Wiki | Ad-hoc |
| `/reindex` | Baut `wiki/index.md` neu auf | Nach Massenänderungen |

### Human-in-the-loop by Design

Der Agent schreibt **niemals** in `raw/`, korrigiert Lint-Issues nie automatisch und archiviert Query-Ergebnisse nicht ohne explizite Anweisung. Jede Schreibentscheidung liegt beim Menschen.

---

## Praxisbeispiel

Dieses Template entstand parallel zu einem konkreten Anwendungsfall:

- **Input:** Ein persönlicher Obsidian-Vault für IT-Berufsausbildung (Netzwerktechnik, Betriebssysteme, IT-Sicherheit, Projektmanagement, IT-Recht)
- **Output:** Ein strukturiertes LLM Wiki — maschinell lesbar, citation-verlinkt, widerspruchserkennend
- **Ergebnis:** Dieselbe Wissensbasis dient sowohl der menschlichen Auswertung (Obsidian-Graph) als auch Agenten-Queries (strukturiertes Wiki-Traversal) — ohne Duplikation

Die zentrale Erkenntnis: **Deine menschlich kuratierten Notizen werden zu strukturiertem Agenten-Input.** Das Wiki ist keine Kopie deiner Notizen — es ist eine kompilierte, normalisierte, cross-verlinkte Repräsentation, optimiert für LLM-Konsum.

---

## Schnellstart

### Voraussetzungen

- [Obsidian](https://obsidian.md/) v1.12+
- [Claude Code](https://claude.ai/claude-code) (aktuelle Version)
- Git (empfohlen)
- Optional: `qmd` für Volltextsuche-Performance bei Scale (>200 Sources)

### Setup

```bash
# 1. Template klonen
git clone https://github.com/Whitefox75/LLM-Wiki-Template.git mein-wiki
cd mein-wiki

# 2. Ordner in Obsidian als neuen Vault öffnen

# 3. In Claude Code ausführen:
/bootstrap
```

Dann eine Source-Datei in `raw/` ablegen und `/ingest <dateiname>` ausführen. Das Wiki wächst automatisch.

Vollständige Anleitung: [SETUP.md](./SETUP.md) | Workflow-Referenz: [WORKFLOW.md](./WORKFLOW.md) | Scale-Limits: [TRADE-OFFS.md](./TRADE-OFFS.md)

---

## Scale

Optimiert für **≤ 200 Sources / ≤ 500 Pages**. Darüber hinaus wird Index-basiertes Retrieval langsamer — siehe [TRADE-OFFS.md](./TRADE-OFFS.md) für Migrationspfade (qmd, GraphRAG).

---

## Lizenz

MIT
