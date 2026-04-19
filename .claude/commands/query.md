---
name: query
description: Frage gegen das Wiki stellen. Liest relevante Pages, synthetisiert Antwort mit Wikilink-Citations. Schreibt NIEMALS automatisch — Filing ist separate User-Entscheidung.
allowed-tools:
  - Read
  - Write(wiki/**)
  - Glob
  - Grep
---

# /query <question>

## WRITE-MODEL
Agent writes the wiki, human curates the raw sources.
- `/query` ist primär eine **Read-Only-Operation**.
- NIEMALS automatisch in `wiki/**` schreiben. Nur auf expliziten User-Befehl via `/file`.
- Falls Log-Entry gewünscht: Nur nach User-Bestätigung.

## Ablauf

### Schritt 1: Index lesen
`wiki/index.md` vollständig lesen. Überblick über verfügbare Pages gewinnen.

### Schritt 2: Relevante Pages identifizieren
- Index-Sektionen auf Relevanz prüfen.
- Titel-Grep: `grep -r -i "<keyword>" wiki/` für Schlüsselbegriffe der Frage.
- Maximal 8–10 Pages für eine Query laden (bei mehr: priorisieren nach Relevanz-Score).

### Schritt 3: Pages vollständig lesen
Alle identifizierten Pages komplett lesen. Frontmatter und Body.

### Schritt 4: Antwort synthetisieren
- Antwort in natürlicher Sprache, strukturiert.
- **Jeder Fact mit Citation:** `[[page-name]]` direkt nach der Aussage.
- Nur auf existierende `wiki/`-Pages verlinken. Keine externen Referenzen in der Antwort.
- Wenn eine Frage nicht beantwortbar ist: Explizit sagen welche Pages fehlen.

### Schritt 5: Filing-Angebot
Am Ende der Antwort immer:
> "Diese Antwort als neue Wiki-Page speichern? → `/file <vorgeschlagener-slug>`"

### Schritt 6: Log-Entry (optional)
Log-Entry nur appenden wenn User explizit bestätigt oder wenn `/file` ausgeführt wird.
Format:
```
## [YYYY-MM-DD HH:MM] query | <short-question>

- Pages consulted: [[page1]], [[page2]]
- Filed as: [[overviews/slug]] oder "none"
```

## Constraints

- Keine externen Referenzen oder URLs in der Query-Antwort.
- Keine Halluzinationen: Wenn das Wiki eine Frage nicht beantworten kann, direkt sagen.
- Kein automatisches Schreiben in `wiki/**` ohne User-Entscheidung.
