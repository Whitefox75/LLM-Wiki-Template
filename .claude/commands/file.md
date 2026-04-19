---
name: file
description: Speichert die vorangegangene Agent-Antwort (typischerweise eine /query-Antwort) als neue Wiki-Page. Updated Index und Log. Nur auf expliziten User-Befehl.
allowed-tools:
  - Read
  - Write(wiki/**)
  - Glob
---

# /file <slug>

## WRITE-MODEL
Agent writes the wiki, human curates the raw sources.
- Dieser Command wird NUR auf expliziten User-Befehl ausgeführt.
- NIEMALS automatisch nach `/query` filen — User muss `/file` explizit aufrufen.
- `raw/**` bleibt unangetastet.

## Ablauf

### Schritt 1: Content bestimmen
- Standard: Die unmittelbar vorangegangene Agent-Antwort (typischerweise eine `/query`-Antwort).
- Falls User explizit Content spezifiziert: diesen verwenden.

### Schritt 2: Page-Typ bestimmen
Anhand des Inhalts:
- Synthese über einen Domain → `wiki/overviews/<slug>.md`
- Vergleich zweier Dinge → `wiki/comparisons/<slug>.md`
- Antwort über ein Concept → `wiki/concepts/<slug>.md`
- Antwort über eine Entity → `wiki/entities/<slug>.md`

Bei Unsicherheit: User fragen oder Default `wiki/overviews/<slug>.md` verwenden.

### Schritt 3: Page schreiben
- Passendes Template aus `_page-templates/` laden.
- Content in Template-Struktur einpassen.
- Frontmatter vollständig ausfüllen (type, created, sources aus den `/query`-Citations).
- Alle `[[citations]]` aus der Query-Antwort beibehalten.

### Schritt 4: Index updaten
Neue Page in die passende Sektion von `wiki/index.md` eintragen.

### Schritt 5: Log-Entry
```
## [YYYY-MM-DD HH:MM] file | <slug>

- Filed as: [[<type>/<slug>]]
- Type: <page-type>
- Source query: "<original question>"
- Citations preserved: [[p1]], [[p2]]
```

### Schritt 6: Bestätigung an User
- Pfad der neuen Page
- Hinweis: Page kann jetzt als Source für zukünftige Queries dienen
