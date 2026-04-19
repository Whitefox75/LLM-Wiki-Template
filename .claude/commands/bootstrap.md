---
name: bootstrap
description: Einmaliger Setup des LLM Wiki Workspace. Legt die vollständige Ordnerstruktur an, erzeugt Seed-Files. Idempotent — existierende Files werden nie überschrieben.
allowed-tools:
  - Read
  - Write(wiki/**)
  - Write(raw/**)
  - Write(_page-templates/**)
  - Bash
  - Glob
---

# /bootstrap

## WRITE-MODEL
Agent writes the wiki, human curates the raw sources.
- `raw/**` ist READ-ONLY für den Agent nach Bootstrap. NIEMALS nachträglich schreiben, editieren, löschen.
- `wiki/**` ist der Working-Area des Agent.
- Jeder Write in `wiki/**` MUSS einen Log-Entry in `wiki/log.md` erzeugen.

## Ablauf

1. Prüfe ob Workspace bereits initialisiert ist (`wiki/log.md` existiert). Falls ja: "Workspace bereits initialisiert. Kein Überschreiben." und stoppe.

2. Erzeuge folgende Ordnerstruktur (nur wenn nicht vorhanden):

```
raw/articles/
raw/papers/
raw/transcripts/
raw/docs/
raw/assets/
wiki/sources/
wiki/entities/
wiki/concepts/
wiki/comparisons/
wiki/overviews/
wiki/lint-reports/
_page-templates/
tools/
.claude/commands/
.claude/skills/llm-wiki/references/
```

3. Erzeuge `wiki/index.md` (Seed) falls nicht vorhanden.
4. Erzeuge `wiki/log.md` (Seed mit Format-Example) falls nicht vorhanden.
5. Erzeuge `.gitignore` falls nicht vorhanden:
   ```
   .DS_Store
   *.pyc
   tools/*.egg-info/
   ```
6. Erzeuge `tools/.gitkeep` falls nicht vorhanden.

7. Appende Log-Entry in `wiki/log.md`:
   ```
   ## [YYYY-MM-DD HH:MM] bootstrap | Workspace initialized
   
   - Folders created: raw/, wiki/, _page-templates/, tools/
   - Seed files: wiki/index.md, wiki/log.md
   ```

8. Gib Report aus: Liste aller erzeugten Ordner und Files, Hinweis auf nächsten Schritt (`/clip <url>` oder manuell in `raw/` ablegen, dann `/ingest`).

## Idempotenz-Regel

Existierende Files oder Ordner werden **nie** überschrieben oder geleert. Zweiter Run ist sicher.
