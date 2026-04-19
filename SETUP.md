# SETUP.md — Installation & Activation

> Full reference: [README.md](./README.md) | Workflow: [WORKFLOW.md](./WORKFLOW.md) | Scale limits: [TRADE-OFFS.md](./TRADE-OFFS.md)

## Requirements

- **Obsidian** ≥ v1.12
- **Claude Code** (current) — or Cowork (equivalent for file operations)
- **Git** (strongly recommended for versioning)
- Optional: `qmd` (`pip install qmd`) for better search performance beyond ~200 pages

## Step 1: Clone the template

```bash
git clone https://github.com/Whitefox75/LLM-Wiki-Template.git my-wiki
cd my-wiki
```

**IMPORTANT:** This workspace is a standalone folder.
Do **not** place it inside an existing personal Obsidian vault.

> Reason (Steph Ango, Obsidian Co-Creator): Keep your personal vault clean.
> Let the agent work in the "messy vault". Only manually carry over curated artifacts.

## Step 2: Start the agent and bootstrap

**Claude Code:**
```bash
claude
/bootstrap
```

**Cowork (Claude Desktop):**
Open the folder in Cowork, then in the chat:
```
/bootstrap
```

The agent creates the full folder structure and outputs a report.

## Step 3: Configure Obsidian

1. Open Obsidian → **Open folder as vault** → select this workspace folder.
2. **Attachment folder:** Settings → Files and links → "Attachment folder path" = `raw/assets/`
3. **Attachment hotkey:** Settings → Hotkeys → "Download attachments for current file" → `Ctrl+Shift+D`

### Recommended plugins

| Plugin | Purpose | Priority |
|---|---|---|
| **Obsidian Web Clipper** | Browser extension for the `/clip` workflow | High |
| **Dataview** | Query pages via frontmatter fields | Optional |
| **Marp** | Render wiki pages as slides | Optional |

Install Web Clipper: [obsidian.md/clipper](https://obsidian.md/clipper) (browser extension)

## Step 4: Test with your first source

**Option A — Clip a URL:**
```
/clip https://example.com/interesting-article
/ingest raw/articles/<slug>.md
```

**Option B — Drop a file manually:**
```bash
cp ~/Downloads/paper.pdf raw/papers/paper.pdf
```
```
/ingest raw/papers/paper.pdf
```

**Option C — Import existing Markdown notes:**
```bash
cp ~/my-vault/note.md raw/articles/note.md
```
```
/ingest raw/articles/note.md
```

## Step 5: Watch Obsidian in parallel

Keep Obsidian open during ingestion. In Graph View (`Ctrl+G`) watch new Entity and Concept nodes appear and get linked in real time.

## Optional: qmd for fast search

Beyond ~200 pages, `qmd` is worth installing for semantic search:

```bash
pip install qmd
qmd index wiki/
```

After that, `/search` uses `qmd` automatically instead of grep.

## Recommended Git workflow

```bash
# After every ingest
git add wiki/
git commit -m "ingest: <source-title>"

# After lint fixes
git add wiki/
git commit -m "lint: fix orphans / contradictions"
```

Protects against silently drifting content (agent-drift). Without Git, agent-drift is undetectable — see [TRADE-OFFS.md](./TRADE-OFFS.md).

## Next steps

After setup:
- Understand the workflow cadence → [WORKFLOW.md](./WORKFLOW.md)
- Know the scale limits → [TRADE-OFFS.md](./TRADE-OFFS.md)
- Run `/lint` after the first dozen ingests

---

---

# SETUP.md — Installation & Aktivierung *(Deutsch)*

> Vollständige Referenz: [README.md](./README.md) | Workflow: [WORKFLOW.md](./WORKFLOW.md) | Scale-Limits: [TRADE-OFFS.md](./TRADE-OFFS.md)

## Voraussetzungen

- **Obsidian** ≥ v1.12
- **Claude Code** (aktuell) — oder Cowork (gleichwertig für Datei-Operationen)
- **Git** (dringend empfohlen für Versioning)
- Optional: `qmd` (`pip install qmd`) für bessere Such-Performance ab ~200 Pages

## Schritt 1: Template klonen

```bash
git clone https://github.com/Whitefox75/LLM-Wiki-Template.git mein-wiki
cd mein-wiki
```

**WICHTIG:** Dieser Workspace ist ein eigenständiger Ordner.
Ihn **nicht** in einen bestehenden persönlichen Obsidian-Vault legen.

> Grund (Steph Ango, Obsidian Co-Creator): Persönlichen Vault sauber halten.
> Agent im "messy vault" arbeiten lassen. Nur kuratierte Artefakte manuell übernehmen.

## Schritt 2: Agent starten und Bootstrap

**Claude Code:**
```bash
claude
/bootstrap
```

**Cowork (Claude Desktop):**
Ordner in Cowork öffnen, dann im Chat:
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
/ingest raw/articles/<slug>.md
```

**Option B — Datei manuell ablegen:**
```bash
cp ~/Downloads/paper.pdf raw/papers/paper.pdf
```
```
/ingest raw/papers/paper.pdf
```

**Option C — Bestehende Markdown-Notizen importieren:**
```bash
cp ~/mein-vault/notiz.md raw/articles/notiz.md
```
```
/ingest raw/articles/notiz.md
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

# Nach Lint-Fixes
git add wiki/
git commit -m "lint: fix orphans / contradictions"
```

Schützt vor unbemerkt driftenden Inhalten (Agent-Drift). Ohne Git ist Agent-Drift nicht nachvollziehbar — siehe [TRADE-OFFS.md](./TRADE-OFFS.md).

## Nächste Schritte

Nach dem Setup:
- Workflow-Kadenz verstehen → [WORKFLOW.md](./WORKFLOW.md)
- Scale-Limits kennen → [TRADE-OFFS.md](./TRADE-OFFS.md)
- `/lint` nach dem ersten Dutzend Ingests ausführen
