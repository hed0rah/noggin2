# Noggin2

A second-brain system for Obsidian and an LLM assistant (Claude). No community plugins, no cloud ingest, no automation platform. Everything is plain markdown in one folder. The assistant catches up behind your capture instead of making you file things.

This README is self-contained. Read it, then copy the seed files (`CLAUDE.md`, `CHEATSHEET.md`, `_templates/daily_template.md`, and the `SETUP.md` prompt) into your own vault.

---

## Why

Second-brain tools fail one of two ways. They over-index on capture, so the vault fills with fragments nobody processes and it rots into a junk drawer. Or they over-index on structure, so filing anything is expensive and you stop capturing at all.

Noggin2 splits the two jobs:

- **Daily notes** are for raw capture with zero ceremony. You write. You do not decide where anything goes.
- **Topic folders** are for processed, linked, durable knowledge.
- **An assistant** bridges the two, using tags as the interface. You never stop to file. The system catches up.

The constraints are deliberate: plain markdown only, readable in vanilla Obsidian on any device forever; the only things that touch the vault are you, Obsidian, and the assistant. The trust boundary is you, Obsidian, the assistant, and the filesystem. Nothing else.

---

## How it works, in one picture

```
you write all day                assistant catches up
------------------               --------------------
2026-05-06.md                    - answers #claude inline (as a footnote)
  #dailymusic ...                - compiles #todo / #tobuy / #reminder to _compiled/
  buy new strings #tobuy         - copies yesterday's note to a themed twin
  fix the amp #todo              - seeds today's CONTINUED from open threads
  who wrote this? #claude        - writes claudespeaks + recall
```

You capture in the stream. The assistant turns it into answers, lists, durable notes, and a running memory of what you are in the middle of.

---

## Three kinds of tag

The whole interface is tags, sorted by how often they fire. Keep the set small; hashtags are easy to over-invent.

### 1. Inline, acted on automatically (frequent)

- `#claude` — you are asking. The assistant answers as a footnote without touching your line. `who painted this #claude` becomes `who painted this #claude[^c-1]` with the answer at the bottom.
- `#todo` — compiled into `_compiled/todos.md`, one rolling checklist. Each item carries a stable block ref `^todo-XXXXXX` (hashed from the text and source) so your check-state survives every rebuild.
- `#tobuy` — a living wishlist, same stable-hash checkboxes.
- `#reminder` — resurfaced later in `## recall`, rotated by relevance to what you have been doing.

### 2. Compiled intake (passive filing)

- `#dailymusic`, `#dailyfilm`, `#dailyquote`, `#dailyart`, `#dailymeme`, `#dailygame` — each tagged line is copied into `_compiled/<name>.md`, a running log per category. Add your own (`#dailybook`, `#dailyskate`).
- `#inspo` — inspiration you grabbed (a link, an image, a reference). Captured to `_compiled/inspiration.md`.
- `#seed` — an idea that is *yours* (a premise, a sketch, a worldbuilding kernel). Compiled per-topic to `<Topic>/seeds.md`. The inspo/seed split exists so you never scroll past your own ideas thinking they are bookmarks. Inspo gets consumed; seeds get developed.

### 3. `#run <verb>` — the on-demand dispatcher

The heavier actions are powerful but infrequent, so instead of a tag each, they live behind one verb tag. Write it in a note or just ask.

| command | what it does |
| --- | --- |
| `#run deepdive <topic>` | a structured research note (frontmatter, sections, sources, wikilinks) |
| `#run rabbithole <topic>` | a verbose exploration note in `_rabbitholes/`, history and tangents and all |
| `#run link [note]` | a linking pass: find related notes, add bidirectional wikilinks |
| `#run title <url>` | fetch the page title, wrap a bare URL as `[Title](url)` |
| `#run promote <what> -> <Folder>` | move daily content into a topic folder, with backlinks both ways |
| `#run project <idea>` | scaffold `_projects/<slug>/brief.md` (what/why, plan, open questions) |
| `#run review <thing>` | a critique pass over a note, plan, draft, or code: gaps, errors, weak spots, open questions |
| `#run digest` | build this week's digest now |
| `#run rebuild <cache>` | regenerate a `_compiled/*.md` file from the notes |

Bare `#deepdive` and `#rabbithole` (no `#run`) stay as quiet "maybe later" markers. `#run deepdive <topic>` is when you want it now.

Anything unrecognized gets logged in `_meta/tag-incubator.md`; recurring tags are candidates to promote.

---

## The daily note lifecycle

- **Today** is a bare-date file, `2026-05-06.md`. You append to it freely all day.
- **The next day**, the assistant copies it to a themed twin, `2026-05-06 broken amp fix.md` (2 to 4 lowercase words, the most memorable thing). All processing happens in the twin; the bare original is frozen, and you delete it once you have glanced at the copy.
- Daily notes are **append-only**. The assistant only adds footnote markers and its own two sections. Past lines are never rewritten; corrections are dated footnotes.

Theme-on-copy is what keeps months of notes browsable. `2026-05-06 broken amp fix` tells you what that day was.

---

## Carryover: the note that remembers what you were doing

The piece that makes it feel like a second brain rather than a filing cabinet. When a new day's note is created, the assistant seeds its `## CONTINUED` section with the threads still open the day before. You open today and it already lists what you were mid-stream on. Each line is terse; the detail hangs in a footnote:

```
## CONTINUED

> from 2026-05-05, terse on purpose. click a footnote for the detail.

- finish the fuzz pedal, box it up[^cf1]
- reply to the label about stems[^cf2]

## footnotes

[^cf1]: germanium fuzz build. next: drill the enclosure, mount pots, test bypass. -- 2026-05-06
```

Finished items drop off. The list shrinks as you close things out.

---

## claudespeaks and recall

Two sections the assistant writes once a day. `## claudespeaks` is one block: a thought or a connection drawn from your recent activity, specific rather than generic. `## recall` surfaces 2 to 3 `#reminder` items, rotated so old reminders resurface when they matter.

---

## Footnotes as a greppable history

The assistant never edits your words. It replies in footnotes keyed by prefix: `[^c-N]` (answers), `[^cf-N]` (carried-forward), `[^remind-N]` (reminders). Every one ends with ` -- YYYY-MM-DD`. So `grep -rn '\[^c-' .` is every answer you ever got, with dates.

---

## The inbox

`_inbox.md` at the root is a friction-free capture buffer. Type one line anywhere, save. The assistant empties it next pass, routing by hint:

```
some fragment                  -> today's daily note (default)
-> Music  some fragment        -> that topic folder
-> append [[Note]]  some text  -> appended to an existing note
-> claude  some question       -> treated as a #claude question
-> defer  something            -> stays in the inbox
```

---

## Weekly digest

On a Sunday (or `#run digest`), the assistant writes `_digests/YYYY-Wxx.md`: intake counts by category, recurring themes, open `#claude` items, promotion candidates, orphan notes. The reader's companion to the writer's daily capture.

---

## Rules the assistant follows

- Copy before editing a past daily note; the bare original is frozen once a themed twin exists.
- Append-only; correct with a dated footnote, never a rewrite.
- Never promote into a topic folder, or invent Maps of Content, unprompted.
- Load a protocol doc only when there is work that needs it.

---

## Obsidian setup

Vanilla, no community plugins. Turn on the core **Daily notes** plugin, point it at your vault, template `_templates/daily_template.md`. Sync is your choice (Obsidian Sync, a git repo, a synced folder); the vault is just markdown.

---

## Build it from scratch

1. Make a vault (a folder) with `_meta/`, `_templates/`, `_compiled/`, `_inbox.md`.
2. Drop `CLAUDE.md` at `.claude/CLAUDE.md`.
3. Drop `_templates/daily_template.md` and rename the lanes to your domains.
4. Paste the `SETUP.md` prompt to the assistant once; it scaffolds the `_meta/` docs and empty caches.
5. Start writing daily notes. Ask for a catch-up pass whenever.

---

## Optional: MCP extensions

The core is vanilla. If you want richer capture, wire in MCP servers (resolve `#dailymusic` to real links, file code repos, query a media server) as optional add-ons, each with its own tag and `_meta/` doc. None of it is required.

---

## Layout

```
YourVault/
  .claude/CLAUDE.md          assistant instructions (this repo)
  CHEATSHEET.md              quick daily reference
  _inbox.md                  friction-free capture buffer
  _templates/daily_template.md
  _meta/                     protocol docs + assistant memory (from SETUP.md)
  _compiled/                 auto-generated logs (todos, tobuy, reminders, daily-*, inspiration)
  _rabbitholes/  _digests/  _projects/    generated by #run verbs
  2026-05-06 broken amp fix.md            themed daily notes
  Topic Folders/             durable, linked knowledge (+ per-topic seeds.md)
```
