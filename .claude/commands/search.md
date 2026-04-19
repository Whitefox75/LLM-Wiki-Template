---
name: search
description: Fulltext-Suche im Wiki. Verwendet qmd wenn verfügbar, sonst grep-Fallback. Read-Only — kein Log-Entry.
allowed-tools:
  - Read
  - Glob
  - Grep
  - Bash
---

# /search <query>

## WRITE-MODEL
Read-Only-Operation. Kein Log-Entry. Kein Write in `wiki/**`.

## Ablauf

### Schritt 1: qmd-Check
```bash
which qmd 2>/dev/null && echo "qmd available" || echo "grep fallback"
```

### Schritt 2a: qmd verfügbar
```bash
qmd search "<query>" wiki/
```
Results formatiert zurückgeben: Pfad, Snippet, Score.

### Schritt 2b: grep-Fallback
```bash
grep -r -i -n "<query>" wiki/ --include="*.md" | head -30
```
Plus Index-Titel-Match:
```bash
grep -i "<query>" wiki/index.md
```

### Schritt 3: Ergebnisse ausgeben
- Relevante Pages mit kurzem Kontext-Snippet.
- Direkter Hinweis auf die nützlichsten Pages.
- Angebot: "Soll ich `/query` mit einer konkreten Frage basierend auf diesen Pages starten?"

## Constraints

- Kein Schreiben.
- Kein Log-Entry.
- Bei leeren Ergebnissen: explizit sagen, welche Suchbegriffe zu keinen Matches führten.
