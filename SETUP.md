# SETUP.md — Installation & Aktivierung

## Voraussetzungen

- **Obsidian** ≥ v1.12
- **Claude Code** (aktuell)
- **Git** (dringend empfohlen für Versioning)
- Optional: `qmd` (`pip install qmd`) für bessere Such-Performance ab ~200 Pages

## Schritt 1: Workspace anlegen

```bash
mkdir mein-llm-wiki
cd mein-llm-wiki
git init
```

**WICHTIG:** Dieser Workspace ist ein eigenständiger Ordner.
Ihn **nicht** in einen bestehenden persönlichen Obsidian-Vault legen.

> Grund (Steph Ango, Obsidian Co-Creator): Persönlichen Vault sauber halten.
> Agent im "messy vault" arbeiten lassen. Nur kuratierte Artefakte manuell übernehmen.

## Schritt 2: Claude Code starten und Bootstrap

```bash
claude
```

Im Claude Code Prompt:

```
/bootstrap
```

Der Agent legt die vollständige Ordnerstruktur an und gibt einen Report aus.

## Schritt 3: Obsidian konfigurieren

1. Obsidian öffnen → **Open folder as vault** → diesen Workspace-Ordner wählen.
2. **Attachment-Folder:** Settings → Files and links → "Attachment folder path" = `raw/assets/`
3. **Attachment-Hotkey:** Settings → Hotkeys → "Download attachments for current file" → `Strg+Shift+D`

### Empfohlene Plugins

| Plugin | Zweck | Priorität |
|---|---|---|
| **Obsidian Web Clipper** | Browser-Extension für `/clip`-Workflow | Hoch |
| **Dataview** | Abfragen über Frontmatter-Felder | Optional |
| **Marp** | Wiki-Pages als Slides rendern | Optional |

Web Clipper installieren: [obsidian.md/clipper](https://obsidian.md/clipper) (Browser-Extension)

## Schritt 4: Ersten Source testen

**Option A — URL clippen:**
```
/clip https://example.com/interessanter-artikel
```
Danach:
```
/ingest raw/articles/<slug>.md
```

**Option B — Datei manuell ablegen:**
```bash
cp ~/Downloads/paper.pdf raw/papers/paper.pdf
```
```
/ingest raw/papers/paper.pdf
```

## Schritt 5: Obsidian parallel beobachten

Obsidian während des Ingest offen lassen. Im Graph-View (`Strg+G`) beobachten,
wie neue Entity- und Concept-Nodes entstehen und verlinkt werden.

## Optional: qmd für schnelle Suche

Ab ~200 Pages lohnt sich `qmd` für semantische Suche:

```bash
pip install qmd
qmd index wiki/
```

Danach nutzt `/search` automatisch `qmd` statt `grep`.

## Empfohlener Git-Workflow

```bash
# Nach jedem Ingest
git add wiki/
git commit -m "ingest: <source-title>"
```

Schützt vor unbemerkt driftenden Inhalten (Agent-Drift).
