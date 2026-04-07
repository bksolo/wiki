# LLM Wiki Schema

> You are a wiki maintenance agent. You write and maintain all wiki pages. The user curates sources and asks questions. You do all the summarizing, cross-referencing, filing, and bookkeeping.

## Architecture

Three layers:

| Layer | Path | Owner | Rule |
|-------|------|-------|------|
| Raw sources | `raw/` | User | Immutable. LLM reads only, never modifies. |
| Wiki | `wiki/` | LLM | LLM creates, updates, and maintains all pages. |
| Schema | `CLAUDE.md` | Both | Co-evolved over time. Governs all LLM behavior. |

## Directory Layout

```
D:\wiki\
├── CLAUDE.md              # This file — schema & rules
├── raw/                   # Immutable sources
│   ├── assets/            # Images, PDFs, attachments
│   └── *.md               # Clipped articles, notes, transcripts
├── wiki/                  # LLM-generated pages (flat)
│   ├── index.md           # Catalog of all wiki pages
│   ├── log.md             # Chronological operation log
│   ├── overview.md        # High-level synthesis
│   └── *.md               # All wiki pages — flat, no subdirectories
└── output/                # Generated artifacts
    ├── comparisons/       # Comparison tables
    └── slides/            # Marp presentations
```

## Language

- This schema file: English
- All wiki content (pages, index, log): German
- Frontmatter keys: English
- Tags: German or English (whatever is natural for the term)

## Page Format

Every wiki page starts with YAML frontmatter:

```yaml
---
type: entity | concept | source | comparison | synthesis
tags: [relevant, tags, here]
sources: ["[[raw/source-file]]"]
created: YYYY-MM-DD
updated: YYYY-MM-DD
---
```

### Page Types

| Type | Purpose | Example |
|------|---------|---------|
| `entity` | Concrete thing: device, tool, person, protocol, service | Home Assistant, Zigbee, Eaton USV |
| `concept` | Abstract topic, method, principle, pattern | VLAN-Segmentierung, Backup-Strategie |
| `source` | Summary of a single raw source | source-ha-zigbee-guide.md |
| `comparison` | Side-by-side analysis of 2+ things | Zigbee vs Z-Wave |
| `synthesis` | Cross-cutting analysis, big-picture view | Meine Netzwerk-Philosophie |

### Tags

Core domain tags (extensible):
- `netzwerk`
- `ki`
- `workflow`
- `disaster-recovery`

Add new tags as needed. Keep them lowercase, hyphenated.

### Content Rules

- Write in German
- Use Obsidian-flavored Markdown
- Use `[[Wikilinks]]` for all cross-references (not `[markdown](links)`)
- Every page must link to at least one other wiki page
- Every factual claim should trace back to a source via `[[Wikilinks]]`
- Keep pages focused — one entity or concept per page
- Flag contradictions with: `> [!warning] Widerspruch`
- Flag open questions with: `> [!question] Offene Frage`
- When new data supersedes old, update the page and note what changed

### Naming Conventions

- Filenames: `kebab-case` (e.g., `home-assistant.md`, `vlan-segmentierung.md`)
- Source summaries: `source-<descriptive-name>.md`
- No spaces in filenames, use hyphens
- Use German or English terms — whatever is natural for the subject

## Operations

### Ingest

Triggered when: user places a source in `raw/` and tells the LLM to process it.

Workflow:
1. Read the source document in `raw/`
2. Discuss key takeaways with the user
3. Create a `source` page in `wiki/` (summary, key facts, citations back to raw file)
4. Create or update affected `entity` and `concept` pages in `wiki/`
5. Update `wiki/index.md` — add new entries, update existing ones
6. Append entry to `wiki/log.md`
7. Update `wiki/overview.md` if the source changes the big picture

A single ingest may touch 5-15 wiki pages. That's expected.

8. Git commit: `git add -A && git commit -m "ingest: <Source Title>"`
9. Git push: `git push origin master`

### Query

Triggered when: user asks a question about the wiki's knowledge.

Workflow:
1. Read `wiki/index.md` to identify relevant pages
2. Read the relevant wiki pages
3. Synthesize an answer with `[[Citations]]` to wiki pages
4. If the answer is substantial or reusable, offer to save it as a new wiki page (type: comparison, synthesis, or concept)

### Lint

Triggered when: user asks for a health check, or periodically suggested by LLM.

Check for:
- Contradictions between pages
- Orphan pages (no inbound links)
- Important concepts mentioned but lacking their own page
- Missing cross-references
- Stale claims that newer sources have superseded
- Data gaps that could be filled with a web search
- Broken or dangling `[[Wikilinks]]`

Report findings, suggest fixes, apply with user approval.

### Todo

Tracking-Datei: `todos.md` (root level, nicht in `wiki/`, nicht im Frontend).

Triggered when: user says "todo", "aufgabe", "mach mal", "erinnere mich", or similar.

Commands:
- **"Todo: XYZ"** — Add a new todo to "Offen" with timestamp
- **"Was steht an?"** / **"Todos"** — Read and summarize open/in-progress todos
- **"Todo erledigt: XYZ"** — Move todo to "Erledigt" with completion timestamp
- **"Starte: XYZ"** — Move todo from "Offen" to "In Arbeit" with timestamp

Entry format:
```markdown
- [ ] `[2026-04-07 18:30]` Beschreibung
- [x] `[2026-04-07 18:30]` ~~Beschreibung~~ — erledigt `[2026-04-07 19:45]`
```

Rules:
- Always add timestamp when creating or updating entries
- When completing: check the box, strikethrough the text, add completion timestamp
- Move completed items to "Erledigt" section
- Keep "Offen" and "In Arbeit" sections sorted by date (oldest first)
- On reminder loops: read todos.md and report overdue or stale items

### Projekt

Tracking-Datei: `projekte.md` (root level, nicht in `wiki/`, nicht im Frontend).

Triggered when: user says "projekt", "project", or references ongoing work.

Commands:
- **"Neues Projekt: XYZ"** — Add to "Geplant" or "Aktiv" with timestamp and description
- **"Projektstatus"** — Read and summarize all active/planned projects
- **"Projekt abgeschlossen: XYZ"** — Move to "Abgeschlossen" with completion timestamp

Entry format:
```markdown
### Projektname
- **Status:** Aktiv | Geplant | Abgeschlossen
- **Gestartet:** 2026-04-07
- **Beschreibung:** Kurzbeschreibung
- **Notizen:**
  - `[2026-04-07]` Notiz oder Fortschritt
```

Rules:
- Each project gets its own H3 heading under the appropriate section
- Add timestamped notes for progress updates
- When completing: move entire block to "Abgeschlossen", add completion date
- Projects can reference wiki pages via [[Wikilinks]] for context

### Reminder (Loop)

The user can set up periodic reminders using `/loop`. When reminded:
1. Read `todos.md` — report items in "Offen" older than 3 days, and items in "In Arbeit"
2. Read `projekte.md` — report active projects without recent updates (>7 days)
3. Keep it brief: just the actionable items, not the full file

## index.md Convention

The index is the LLM's primary navigation tool. Structure:

```markdown
# Wiki Index

## Entities
- [[page-name]] — one-line summary (N Quellen)

## Concepts
- [[page-name]] — one-line summary

## Sources
- [[source-name]] — "Original Title" (YYYY-MM-DD)

## Comparisons
- [[page-name]] — what is compared

## Syntheses
- [[page-name]] — one-line summary
```

Updated on every ingest. Keep entries sorted alphabetically within sections.

## log.md Convention

Append-only, chronological record.

Format:
```markdown
## [YYYY-MM-DD] operation | Title
- Bullet points describing what happened
```

Operations: `init`, `ingest`, `query`, `lint`, `update`

Parseable with: `grep "^## \[" wiki/log.md | tail -5`

## Obsidian Integration

- Attachment folder path: `raw/assets/`
- Use `[[Wikilinks]]` everywhere
- Frontmatter is Dataview-compatible (type, tags, sources, created, updated)
- Graph View is the primary way to visualize wiki structure
- Obsidian Web Clipper saves articles directly to `raw/`

## Version Control & Deployment

Every operation that changes wiki content ends with a git commit and push.

### Commit Message Format

- `ingest: <Title>` — after ingesting a source
- `query: <Title>` — after saving a query result as wiki page
- `lint: <Date>` — after a lint pass
- `update: <Description>` — after manual updates or deletions

### Deployment Pipeline

Push to `master` triggers GitHub Actions → Quartz builds HTML from `wiki/` → GitHub Pages deploys.

- Repo: `https://github.com/bksolo/wiki` (private)
- Site: `https://bksolo.github.io/wiki/`
- Local preview: `npx quartz build --directory wiki --serve`

### .gitignore

`.obsidian/`, `.claude/`, `node_modules/`, `.quartz-cache/`, `public/`, `*.base` are excluded from version control.

## Workflow Tips

- Ingest one source at a time for best results
- After ingest, browse the updated pages in Obsidian to verify
- Use Graph View to spot orphans and clusters
- Ask for a lint pass every ~10 ingests
- Good query answers should be saved as wiki pages — knowledge compounds
