# Noggin2: A Second-Brain System for Obsidian + LLM Assistant

A complete guide to building and running a personal second-brain system using Obsidian and a local LLM assistant. No community plugins. No cloud ingest. Everything is plain markdown in one folder. Designed to survive ten years of daily use without ever requiring a migration.

This document is self-contained. Read it to understand the philosophy, then follow the setup section to build your own vault from scratch.

---

## Philosophy

Second-brain tools usually fail for one of two reasons. Either they over-index on capture (the user fills the vault with fragments that never get processed, and it becomes a digital junk drawer) or they over-index on structure (the user never captures anything because the ceremony of filing it is too expensive).

Noggin2 splits the two jobs. Daily notes are for raw capture with zero ceremony. Topic folders are for processed, linked, durable knowledge. An LLM assistant bridges the two using tags as the interface. You never have to stop and think about where something goes. Just write. The system catches up.

The constraints are intentional:

- No plugins. Only Obsidian core features plus Obsidian Sync (optional, paid, phone to desktop). The vault must be readable and editable in plain Obsidian on any device.
- No external services beyond Obsidian Sync. No cloud LLM ingest of the vault, no automation platforms, no webhooks. The only things that touch the vault are you, Obsidian, and the LLM assistant running locally.
- Plain markdown. Everything the assistant produces stays as markdown so it is portable and Obsidian-native.
- Manual backups to external HDD. You own recovery.

The trust boundary is: you, Obsidian, the LLM assistant, and the filesystem. Nothing else.

---

## Folder Layout

```
YourVault/
  CHEATSHEET.md               quick reference for daily use
  _inbox.md                   friction-free capture buffer
  _meta/                      protocol docs and assistant memory
    README.md                 session-start checklist, tag conventions
    SYSTEM.md                 full system overview (shareable)
    voice.md                  observations on your writing style
    linking.md                wikilink methodology
    rename.md                 daily note filename convention
    promote.md                topic-folder promotion workflow
    compile.md                intake compilation protocol
    digest.md                 weekly digest protocol
    moc.md                    Maps of Content protocol
    inbox.md                  inbox routing protocol
    footnotes.md              footnote annotation protocol
    seed.md                   creative seed tag protocol
    todo.md                   todo aggregator protocol
    claudecode.md             claudecode tag protocol
    projects.md               project tracker protocol
    decisions.md              append-only design decisions log
    future.md                 deferred ideas, not yet built
    askclaude/
      README.md               askclaude tag protocol
      log/
        rolling.md            assistant's run log
  _templates/
    daily_template.md         daily note template
  _compiled/                  auto-generated intake logs
    music.md                  all #dailymusic entries
    film.md                   all #dailyfilm entries
    quotes.md                 all #dailyquote entries
    art.md                    all #dailyart entries
    inspiration.md            all #inspo sections
    todos.md                  living checkbox list from #todo tags
  _digests/                   weekly digests (YYYY-Wxx.md)
  _maps/                      Maps of Content (hand-curated hubs)
  _projects/                  project tracker and Claude Code briefs
    _index.md                 project dashboard
  Music Knowledge/            topic folder
  Literature/                 topic folder
  Hardware/                   topic folder
  Design/                     topic folder
  Ai/                         topic folder
  Video Games/                topic folder
  Sculpture_Architecture/     topic folder
  ...                         add your own
```

Every topic folder contains an `_index.md` landing note that describes the folder's scope and companion tags. This file keeps the folder alive through Obsidian Sync (which tracks files, not folders), gives newcomers a landing page, and can grow into a Map of Content later.

Hide from graph view: `_meta/`, `_compiled/`, `_digests/`, `_templates/`, `_inbox.md`

---

## Daily Notes

One per day. Today is always bare `YYYY-MM-DD.md`. After the day rolls over, the assistant adds a 2-4 word topic suffix: `2026-04-11 ripleys seiko.md`. The topic is the single most memorable or unusual thing in the note, not the most important. "shrek 2 bootleg" beats "movies". This gives graph view actual memory hooks instead of a wall of dates.

Template at `_templates/daily_template.md` gets auto-filled by Obsidian's core Daily Notes plugin. A reasonable starting template:

```
#dailymusic 
#dailyquote 
#dailyfilm 
#dailyart 

---

## Projects

- 
- 
- 

---

## Learning

- 
- 
- 

---

found: 

---

#todo 

---



---
```

The daily intake tags at the top force you to ingest something interesting every day, starting with music as the lowest friction entry point. `## Projects` and `## Learning` are heading prompts that get you thinking about what you touched and what you absorbed, without requiring any particular format. "found" is an absolute lowest capture bar for anything worth noting. Pre-loaded `#todo` section for tasks. The blank zone at the end is for freeform dump. Adapt the template to your own habits.

Daily notes are append-only. The assistant never rewrites the body. Its edits are limited to appending footnote markers (`[^c-1]`) and definitions in a `## footnotes` section at the bottom, or adding a small `> promoted: [[Target]]` footer when material gets lifted into a topic folder.

---

## Tags

Tags are the interface between you and the assistant. Drop a tag, the assistant acts on it next time it runs.

### Tags the assistant acts on

`#askclaude` - Ask a question inline. The assistant answers via footnote (simple case) or creates a new topic-folder note and references it from a short footnote (complex case). The original line is never rewritten beyond the appended footnote marker.

`#link-me` - Request a linking pass. The assistant finds related notes via grep, adds wikilinks inline or in a `Related:` footer, and makes them bidirectional.

`#todo` - Compiled into `_compiled/todos.md` as a checkbox list with stable identities (so check state survives regeneration). Check the box to mark done. Add `#todo-done` to the source line to archive it out of the list entirely.

`#inspo` - Section captured into `_compiled/inspiration.md`. Drop alongside a domain tag (`#Design`, `#Hardware`, etc) for filtering. Captures from the tag line down to the next `---` or next tag-prefixed line.

`#seed` - Your own creative fragments (story ideas, premises, character sketches). Compiled per-topic into `Topic/seeds.md`. Same section capture as `#inspo` but routes by co-occurring domain tag. Seeds are things you generated. Inspo is things you grabbed. They compile separately because they want different treatment: inspo gets returned-to-and-consumed, seeds get returned-to-and-developed.

`#claudecode` - Drop a build idea for a Claude Code project. The assistant creates `_projects/<slug>/brief.md` with a structured project plan ready to copy directly into a Claude Code session. Same footnote-marker workflow as `#askclaude`.

`#title-me` - Fetch a page title for a raw URL and replace the line with a titled markdown link. Drop it next to any raw URL you pasted in a hurry. The assistant fetches the page, grabs the title, and swaps in `[Title](url)`. Tag disappears on success. If the page can't be fetched, tag stays for retry and a footnote explains why.

`#dailymusic` / `#dailyfilm` / `#dailyquote` / `#dailyart` - Line compiled into the corresponding `_compiled/*.md` file. These live paired with existing base tags (`#Music`, `#Movie`, `#Quote`, `#Art`) at the top of daily notes. The base tag is your domain marker. The daily tag is the compile trigger.

### Tags the assistant never touches

`#rabbithole` - Your bookmark for things to dig into later. Greppable, not processed.

`#deepdive` - Your marker next to a link that goes deep. Not a request to the assistant.

`#followup` - Revisit later. Same deal.

`#Brightidea` - Your marker for ideas worth developing.

`#todo-done` - Your signal that a todo is closed. The assistant just filters it out on next regen.

All your domain tags (`#Music`, `#Art`, `#Design`, `#Hardware`, `#Literature`, `#philosophy`, etc) are yours. The assistant reads them for context and co-occurrence but never processes them independently.

---

## Footnotes

Footnotes are the annotation layer. They keep the main body of a note pristine while carrying commentary, sources, corrections, digressions, and assistant replies underneath. Obsidian renders them natively: `[^marker]` inline, `[^marker]: definition` at the bottom. Ctrl-click a marker to jump to its definition.

Every footnote marker uses a namespaced prefix so retrospective grep queries work:

`[^c-N]` - Assistant's reply to an `#askclaude` tag. Example: `[^c-1]`

`[^src-slug]` - Source or provenance citation. Example: `[^src-wikipedia]`

`[^YYYY-MM-DD-slug]` - Dated future-you annotating past-you. Example: `[^2026-04-12-wrong]`

`[^dig-N]` - Digression you pulled out of (the rabbit hole you captured but did not go down). Example: `[^dig-1]`

`[^def-word]` - Glossary or etymology drop. Example: `[^def-qid]`

`[^note-slug]` - General side note. Example: `[^note-fuzz-pedals]`

Definitions live under a `## footnotes` section at the bottom of each note. The assistant creates this section when adding its first footnote to a file.

The key rule: never edit a past line to match new information. If something you wrote turns out to be wrong, add a dated footnote. The original thought stays intact with its original energy. The correction is a layer on top. The archaeology matters.

This is the commonplace-book pattern. You get to be two people having a conversation across time.

### Grep patterns for retrospective queries

```
# every assistant reply ever
grep -rn --include="*.md" -E '^\[\^c-' .

# every dated future-self annotation
grep -rn --include="*.md" -E '^\[\^\d{4}-' .

# every digression capture
grep -rn --include="*.md" -E '^\[\^dig-' .

# every source citation
grep -rn --include="*.md" -E '^\[\^src-' .
```

---

## Workflows

### Ask the assistant a question

Drop `#askclaude` anywhere in a note with your question:

```
#askclaude what is the name of that Raymond Scott sequencer with rotating contacts
```

The assistant appends `[^c-1]` and puts the answer in `## footnotes`:

```
#askclaude what is the name of that Raymond Scott sequencer[^c-1]

## footnotes

[^c-1]: the Circle Machine, built ~1959 as part of the Electronium project. 
Rotating disc with adjustable photocells that trigger tones in sequence. 
Direct ancestor of the step sequencer. -- 2026-04-12
```

If the answer is big enough for its own note, the footnote is a one-liner with a wikilink:

```
[^c-1]: full list in [[Experimental Sculpture]]. 4 new kinetic sculptors added. -- 2026-04-12
```

### Drop inspiration

```
#inspo #Design tunnel books / accordion style 3d
https://rarebooks.uflib.ufl.edu/research-teaching/tunnel-books/
https://x.com/historichub/status/2036753542845989121
```

The section from `#inspo` to the next `---` gets captured into `_compiled/inspiration.md`. Filter later with Obsidian search: `path:_compiled/inspiration tag:#Design`.

### Drop a creative seed

```
#seed #Literature A snail baby lives its life, growing older and longer,
climbing the ever lengthening staircase that is its shell.
```

Compiled into `Literature/seeds.md`. Seeds are what you generated. Inspo is what you grabbed. The split exists because scrolling past your own ideas thinking they are someone else's links is a failure mode.

### Capture a todo

```
#todo build a compiler that mimics the genome
```

Shows up in `_compiled/todos.md` as a checkbox:

```
- [ ] build a compiler that mimics the genome  ^todo-27840b
```

The `^todo-27840b` is an Obsidian block reference derived from a hash of the text and source filename. It is stable across regenerations, so your check state persists.

### Promote daily-note content to a topic folder

Ask the assistant explicitly: "promote the seiko stuff from today into Design/". The assistant creates or appends to a note in the target folder, adds a `Source: [[daily-note]]` header, and adds a small `> promoted: [[Target]]` footer in the daily note. The daily-note body is never rewritten.

Promotion is never automatic. Always on your request.

### Request a linking pass

Drop `#link-me` in a note. The assistant finds related notes via grep, adds wikilinks inline or in a `Related:` footer, and makes them bidirectional: if A links to B, B gets a link back to A.

### Quick capture from anywhere

Open `_inbox.md`. Type one line. Save. The assistant empties it next session, routing by hints:

```
some fragment                          -> today's daily note (default)
-> Music Knowledge  some fragment      -> that topic folder
-> append [[Note Name]]  some text     -> appends to an existing note
-> askclaude  some question            -> treated as a question
-> defer  something for later          -> stays in inbox
```

### Regenerate compiled files

Ask "rebuild the compiled files" or "update todos.md". The assistant regenerates from scratch by grepping daily notes. Compiled files are caches. The daily notes are always the source of truth.

### Weekly digest

Auto-generated Sunday morning at `_digests/YYYY-Wxx.md`. Summary of the week's daily notes, intake counts by category, recurring themes, unresolved askclaude items, promotion candidates, and orphan notes. It is the reader's companion to the writer's daily capture.

### Rename old daily notes

Ask "rename the daily notes that need topics". The assistant grabs everything earlier than today, picks topics (most memorable/unusual thing), updates any internal wikilinks first, then renames. Current day is never touched.

### Title a raw URL

```
#title-me https://www.luhringaugustine.com/exhibitions/emily-kraus
```

Becomes: `[Emily Kraus - Luhring Augustine](https://www.luhringaugustine.com/exhibitions/emily-kraus)`. Works with surrounding text too (your words stay, the URL gets wrapped). If the page can't be fetched, tag stays and you get a footnote explaining why.

### Kick off a Claude Code project

Drop `#claudecode` in a daily note with a build idea:

```
#claudecode build a second-brain backup system that syncs vault to IPFS
```

The assistant creates `_projects/<slug>/brief.md` with: project what/why, architecture, implementation plan, dependencies, open questions, and a ready-to-paste Claude Code session prompt. The brief is designed so you can copy it straight into a Claude Code session and start building immediately.

### Check your project dashboard

Open `_projects/_index.md`. The dashboard shows all projects across five tiers: active, queued, stale, sparks, and shipped. Each entry includes the project name, last-seen date with source tag, and a quick summary. Run this before standing up new work to surface anything you may have forgotten.

---

## The Todo Aggregator

`_compiled/todos.md` is a living checkbox list compiled from every `#todo` line in the vault. Each item carries a stable Obsidian block reference (`^todo-XXXXXX`) derived from a hash of the text and source, so your check state survives regeneration.

Two ways to mark done:

1. Check the box in the collector. Item moves to `## completed` on next regen.
2. Add `#todo-done` to the line in the daily note. Item disappears from the collector entirely.

Active items are grouped by source note, sorted by date descending (most recent at top). Completed items collect at the bottom.

---

## The Inspo/Seed Split

Two tags for two kinds of "things worth coming back to":

`#inspo` is external. A link, an image, a book recommendation, a design reference. Origin is outside you. Compiles into one global file: `_compiled/inspiration.md`.

`#seed` is internal. A story fragment, a premise, a character sketch, a worldbuilding kernel. Origin is you. Compiles per-topic: `Literature/seeds.md`, `Hardware/seeds.md`, etc.

Both use section capture (tag line to next `---`). Both use co-occurring domain tags for filtering. The split exists because they want different downstream treatment. Inspo gets consumed. Seeds get developed. Conflating them means you scroll past your own ideas thinking they are bookmarks.

---

## Project Tracker

Your build ideas live in `_projects/_index.md`, a five-tier dashboard aggregating projects across the vault. Tiers: active (actively being developed), queued (ready when bandwidth opens), stale (no signal in 14+ days), sparks (raw ideas), and shipped (completed work).

Each project carries a `last seen: YYYY-MM-DD` date with a source tag indicating where the signal came from (vault mention, session transcript, machine directory scan, GitHub activity).

The tracker uses live signal sources to detect project health. These include vault grep (daily note mentions and `#Brightidea` tags), session transcripts (mentions in interactive work), directory scanning of ~/Projects on any machine the assistant can reach via SSH, and GitHub repo activity. On every run, the assistant checks all sources and updates project visibility. Projects with no signal across any source in 14+ days auto-move to stale tier. Stale means "you probably forgot about this", not dead.

Deduplication during compile passes ensures that mentions of the same project across different notes and sources are rolled up into a single tracker entry.

Weekly digest includes a project health section reporting which projects moved tiers and why.

---

## The Decisions Log

`_meta/decisions.md` is an append-only record of every design decision about the system: the rule, the reason, and which doc codifies it. New entries go on top. Old entries are never edited or deleted.

The point of the log is to stop rules from drifting. If six months from now you wonder "why does the assistant not rename today's daily note?", the answer lives in the decisions log with the reason, not just a codified rule you no longer remember agreeing to.

When changing a decision, add a new entry on top. The old entry stays. The history is the point.

---

## Voice Preservation

You write in fragments, tag-prefixed sections, raw URLs, image embeds. The assistant does not prose-ify your writing. When promoting material out of a daily note into a topic folder, the raw fragments stay raw; structure and links are added around them. The assistant's own content is kept visually distinct (footnotes under a `## footnotes` section, namespaced marker prefixes like `[^c-1]`) so you always know who wrote what.

The assistant maintains a `_meta/voice.md` file with observations about your writing style, recurring patterns, and structural conventions. It updates this file as new patterns emerge. This is how the assistant matches your tone when composing new content in topic folders.

---

## Maps of Content

`_maps/` holds hand-curated hub notes that collect wikilinks to the best material in the vault on a single theme. Examples: `Audio and Synthesis.md`, `Hardware Hacking.md`, `Sci-fi Worldbuilding.md`.

MOCs are reader's indexes. Topic folders are library stacks. Graph view clusters around MOCs as hub nodes.

The assistant never autonomously creates MOCs. They represent your editorial judgment about what matters. The assistant only edits them on your explicit request.

---

## Rules

1. Today's daily note is never renamed. Topic suffix gets added the next day or later.
2. Daily notes are never rewritten. The assistant's edits are surgical (footnotes only).
3. Promotion to topic folders is always user-requested, never automatic.
4. Compiled files are caches. Regenerate from scratch. Never hand-edit (except checkbox state in todos).
5. `#daily*` tags are paired with base tags, not replacing them. Both stay.
6. Scheduled runs are grep-first: zero work means one grep and one log line. No wasted tokens.
7. Screenshots can be OCR'd and removed. Handwritten content stays unless explicitly told otherwise.
8. Never edit a past line to correct it. Add a dated footnote instead.
9. No community plugins, no cloud LLM ingest, no automation platforms. Obsidian core + Sync + local assistant. That is the trust boundary.

---

## Obsidian Setup

All core plugins, zero community plugins.

**Daily Notes plugin:** template path `_templates/daily_template`, date format `YYYY-MM-DD`, new file location at vault root.

**Templates plugin:** for manual template insertion via hotkey.

**Outline, Graph, Backlinks:** core navigation.

**Excluded from graph:** `_meta/`, `_compiled/`, `_digests/`, `_templates/`, `_inbox.md`

**Obsidian Sync** (optional, paid): phone to desktop replication. The system works without it, but Sync is how phone captures reach the desktop vault.

---

## Assistant Operating Model

The assistant runs in two contexts:

**Interactive session.** When you open a session, the assistant does a cheap check: grep for pending `#askclaude` and `#link-me` tags. Zero matches means it stops the routine and handles whatever you came to do. Tags found means it reads the relevant protocol doc lazily (only when there is actual work), processes the tags, and briefly reports what was done.

**Scheduled run.** A daily task at 6AM local time runs the same grep-first protocol. On zero-work days it does one grep and one log-append and exits. On days with work, it reads the protocol docs, processes tags, and logs results to `_meta/askclaude/log/rolling.md`. A separate Sunday morning task generates the weekly digest.

Token usage is a design constraint. The processing logic lives in `_meta/askclaude/README.md` so the high-level README can be skipped on most runs. `voice.md` is only loaded when the assistant is actually composing new content.

The scheduled run does NOT do autonomous full-vault linking passes or promotions. Those are user-requested only.

---

## How to Build This From Scratch

1. Install Obsidian. Create a vault at a path you own.
2. Optionally buy Obsidian Sync for cross-device replication.
3. Create the folders:
   - `_meta/`
   - `_meta/askclaude/`
   - `_meta/askclaude/log/`
   - `_templates/`
   - `_compiled/`
   - `_digests/`
   - `_maps/`
   - Plus whatever topic folders you care about (start with 3-5, add as you go)
4. Create `_templates/daily_template.md` with your daily intake tags.
5. Create `_inbox.md` at the vault root (empty file).
6. Create `_meta/decisions.md` (empty log with the header pattern).
7. Create `_meta/voice.md` (seed it with a few observations about how you write).
8. Seed each topic folder with an `_index.md` landing note describing its scope and companion tags.
9. Create empty compiled files: `_compiled/music.md`, `film.md`, `quotes.md`, `art.md`, `inspiration.md`, `todos.md`.
10. Enable core plugins: Daily Notes (set template path, filename format `YYYY-MM-DD`), Templates, Outline, Graph, Backlinks.
11. Set the graph exclusions: `_meta/`, `_compiled/`, `_digests/`, `_templates/`, `_inbox.md`.
12. Set up a Claude cowork session (or any local LLM agent) pointed at the vault folder with read/write access.
13. Configure a daily 6AM scheduled task: grep for pending tags, process `_inbox.md`, process tags, log results.
14. Configure a Sunday morning digest task.
15. Start capturing. Drop tags wherever. Let the system catch up.

The whole system fits in one folder, one assistant, and one recurring schedule. That is the point.

---

## Key Protocol Files

If you replicate this system, these are the docs the assistant reads at runtime. You do not need to read them all, but they are the specification:

`_meta/README.md` - Session-start checklist, full tag conventions, operating principles.

`_meta/SYSTEM.md` - Full system overview. Shareable.

`_meta/askclaude/README.md` - How the assistant processes `#askclaude` tags.

`_meta/footnotes.md` - Footnote annotation layer: all six prefixes, compile interactions, placement rules.

`_meta/seed.md` - `#seed` tag protocol, compile routing, the inspo/seed distinction.

`_meta/todo.md` - Todo aggregator: hash-based stable identities, checkbox persistence, completion paths.

`_meta/claudecode.md` - `#claudecode` tag protocol, project brief structure, Claude Code session prompts.

`_meta/title-me.md` - `#title-me` tag protocol, URL title fetching, failure handling.

`_meta/projects.md` - Project tracker dashboard, five tiers, live signal sources, stale detection, deduplication.

`_meta/compile.md` - How `_compiled/*.md` files are generated from daily-note tags.

`_meta/rename.md` - Daily note filename convention, topic selection criteria, current-day rule.

`_meta/linking.md` - Wikilink methodology, when to link, bidirectional rules.

`_meta/promote.md` - Topic-folder promotion workflow.

`_meta/digest.md` - Weekly digest generation.

`_meta/moc.md` - Maps of Content protocol.

`_meta/inbox.md` - Inbox routing protocol.

`_meta/voice.md` - Your writing style observations.

`_meta/decisions.md` - Why the rules are the way they are. The log that stops drift.

`_meta/future.md` - Ideas considered but deferred, with revisit conditions.

---

## When in Doubt

Just capture. Drop it in a daily note or `_inbox.md` with whatever tags feel right. File-friction is the enemy of a second brain. You and Sunday-you will sort it out later.
