---
name: clip
description: Fetcht Inhalt einer URL, konvertiert zu Markdown, speichert in raw/articles/<slug>.md. Kann direkt anschließend /ingest triggern. Einstiegspunkt für Web-Content.
allowed-tools:
  - Read
  - Write(raw/articles/**)
  - Write(wiki/log.md)
  - Bash
  - WebFetch
---

# /clip <url>

## WRITE-MODEL
Agent writes the wiki, human curates the raw sources.
- `/clip` schreibt EINMALIG in `raw/articles/` — das ist der einzige Fall, wo der Agent in `raw/` schreibt.
- Nach dem Clip ist `raw/articles/<slug>.md` READ-ONLY für den Agent (wie alle anderen `raw/`-Files).
- Kein nachträgliches Editieren der geclippen File.

## Ablauf

### Schritt 1: URL fetchen
WebFetch-Tool auf `<url>` aufrufen. Full-Content-Modus.

### Schritt 2: Slug erzeugen
Aus dem `<title>`-Tag oder H1 der Page: kebab-case, max. 60 Zeichen.
Beispiel: `andrej-karpathy-llm-wiki-gist`

### Schritt 3: Markdown konvertieren
- HTML → Markdown (Headings, Links, Code-Blocks erhalten).
- Navigation, Footer, Ads, Cookie-Banner entfernen.
- Images: Alt-Text erhalten, `![alt](raw/assets/<filename>)` als Platzhalter.

### Schritt 4: Source-Frontmatter schreiben
```yaml
---
type: raw-source
title: "<title>"
source_url: "<url>"
fetched_at: "YYYY-MM-DD HH:MM"
---
```

### Schritt 5: File speichern
`raw/articles/<slug>.md` schreiben.

### Schritt 6: Log-Entry
```
## [YYYY-MM-DD HH:MM] clip | <title>

- Saved: raw/articles/<slug>.md
- Source URL: <url>
- Size: ~N Wörter
```

### Schritt 7: Ingest-Angebot
```
Geclipt: raw/articles/<slug>.md (~N Wörter)
Direkt ingestionieren? → /ingest raw/articles/<slug>.md
```

User entscheidet. Kein automatischer Ingest.

## Fehlerbehandlung

- URL nicht erreichbar: Fehlermeldung ausgeben, kein File schreiben.
- Paywall / Login-Wall: Hinweis, dass manuelles Ablegen in `raw/` nötig ist.
- Sehr langer Content (>10.000 Wörter): Warnung ausgeben, trotzdem komplett clippen.
