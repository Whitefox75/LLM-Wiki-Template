# LLM Wiki Workspace

Ein **Karpathy-Style LLM Wiki** — ein persistentes, compoundierendes Wissens-Artefakt,
das ein LLM-Agent inkrementell aus unveränderlichen Rohquellen aufbaut und pflegt.

## Konzept

> "Wiki statt RAG: Wissen wird einmal kompiliert und dann current gehalten —
> nicht bei jeder Query neu aus Chunks rekonstruiert." — Andrej Karpathy

## Schnellstart

```bash
# 1. Claude Code im Workspace-Root starten
# 2. Bootstrap ausführen
/bootstrap

# 3. Erste Source clippen und einarbeiten
/clip https://example.com/article
/ingest raw/articles/article.md

# 4. Frage stellen
/query "Was sagt die Source über X?"
```

## Struktur

```
raw/        ← Deine Quellen (immutable, read-only für den Agent)
wiki/       ← Agent-owned Wiki (auto-maintained)
CLAUDE.md   ← Schema-Dokument (lesen vor erstem Einsatz)
```

## Commands

| Command | Zweck |
|---|---|
| `/bootstrap` | Einmaliger Setup |
| `/ingest <path>` | Source einarbeiten |
| `/query <question>` | Wiki abfragen |
| `/lint` | Health-Check |
| `/search <query>` | Fulltext-Suche |
| `/reindex` | Index neu aufbauen |
| `/file <slug>` | Antwort als Wiki-Page speichern |
| `/clip <url>` | URL zu Raw-Source konvertieren |

## Dokumentation

- `SETUP.md` — Installation, Obsidian-Konfiguration
- `WORKFLOW.md` — Kadenz und Compounding-Loop
- `TRADE-OFFS.md` — Scale-Limits, bekannte Schwächen
- `CLAUDE.md` — Schema-Dokument (für den Agent)

## Quelle

Andrej Karpathy — [llm-wiki.md](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) (April 2026)
