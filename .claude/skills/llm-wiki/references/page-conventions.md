# Page-Conventions — Frontmatter, Links, Headings

## Frontmatter-Regeln

- **Pflicht:** Jede Page hat Frontmatter. Kein File ohne `type`-Field.
- **Templates:** Immer aus `_page-templates/<type>.md` starten. Keine freien H2-Headings erfinden.
- **Dates:** ISO-Format `"YYYY-MM-DD"` oder `"YYYY-MM-DD HH:MM"`. Immer in Anführungszeichen (YAML-Kompatibilität).
- **Arrays leer:** `[]` statt wegzulassen — Frontmatter-Parser-Kompatibilität.
- **evergreen:** `false` als Default. `true` nur explizit setzen für zeitlose Definitionen (z.B. mathematische Theoreme).

## Link-Konventionen

### Standard-Format
```
[[EntityName]]          ← kurz, Obsidian resolvt automatisch
[[concepts/name]]       ← explizit bei Ambiguität
[[sources/slug]]        ← immer explizit für Sources (Eindeutigkeit)
```

### Wann expliziter Pfad
- Zwei Pages mit gleichem Namen in verschiedenen Sektionen → expliziter Pfad Pflicht.
- In Frontmatter-Arrays → immer expliziter Pfad.
- In Comparison-Subjects → expliziter Pfad.

### Citation-Pflicht
Jeder Fact-Claim in einer Wiki-Page muss entweder:
- Mit `[[sources/slug]]` verlinkt sein, oder
- Unter `## Open Questions` stehen.

**Keine unbelegten Behauptungen.**

## Heading-Regeln

- **H1:** Nur einmal pro Page. Entspricht `name` oder `title` aus Frontmatter.
- **H2:** Nur die im Template definierten Headings. Keine freien H2 erfinden.
- **H3+:** Frei verwendbar innerhalb von Template-Sektionen.
- Schema-Evolution: Neue H2-Headings nur nach expliziter Änderung in `CLAUDE.md` + `_page-templates/`.

## Naming-Conventions

| Page-Typ | Naming | Beispiel |
|---|---|---|
| Source | kebab-case vom Titel | `karpathy-llm-wiki-gist` |
| Entity | PascalCase | `AndrejKarpathy`, `OpenAI` |
| Concept | kebab-case | `compile-once-maintain-forever` |
| Comparison | `<a>-vs-<b>` kebab | `wiki-vs-rag` |
| Overview | Domain kebab-case | `large-language-models` |

## Entity-Disambiguation

Bei gleichnamigen Entities → Suffix-Konvention:
```
Apex-Software
Apex-Hardware
```
In `aliases`-Frontmatter den Originalnamen eintragen:
```yaml
aliases: ["Apex"]
```

## Page-Größen-Limit

- **Max. 1.500 Wörter** pro Page.
- Längere Pages in Sub-Concepts aufteilen (neue Concept-Page anlegen, verlinken).
- Source-Pages: 200–500 Wörter Summary. Kompakt bleiben.

## Zitat-Regeln (Copyright)

- Max. **1 Quote pro Source-Page**.
- Max. **15 Wörter** pro Quote.
- Immer in `> `-Blockquote.
- Längere Passagen: paraphrasieren, nicht zitieren.

## Sprache

- Pages werden in der Sprache der dominanten Source verfasst.
- Bei Mixed-Language-User: vor erstem Ingest fragen.
- Sprachmix innerhalb einer Page vermeiden.
- Technische Terme (Englisch) sind auch in deutschen Pages akzeptabel.
