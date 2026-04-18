# Noggin2 Setup Prompt

Copy everything below the line into a fresh Claude Cowork session. Before pasting, select your Obsidian vault folder so Cowork has read/write access to it. The vault should already exist in Obsidian (File > Open vault > create one if needed). The assistant will build the full system scaffold inside it.

---

I want you to build a second-brain system called Noggin2 inside the vault folder I just selected. This system uses Obsidian (core plugins only, zero community plugins) plus you as the LLM assistant. Everything is plain markdown.

The system has two layers: daily notes for raw capture with zero ceremony, and topic folders for processed durable knowledge. Tags are the interface between me and you. I drop tags, you act on them next time you run.

Please do ALL of the following in order. Do not ask me questions, just build it. I will customize afterward.


## 1. Create the folder structure

```
_meta/
_meta/askclaude/
_meta/askclaude/log/
_templates/
_compiled/
_digests/
_maps/
_projects/
_rabbitholes/
```

Plus these starter topic folders (I will add more later):

```
Music Knowledge/
Literature/
Hardware/
Design/
Ai/
Video Games/
```


## 2. Create _inbox.md at vault root

A friction-free capture buffer. Content:

```
<!-- Noggin2 inbox. Dump anything here. The assistant routes it on next run.
Hints: -> daily (default), -> Music Knowledge, -> append [[Note]], -> askclaude, -> defer -->
```


## 3. Create _templates/daily_template.md

```
#dailymusic 
#dailyquote 
#dailyfilm 
#dailyart 

---

## claudespeaks


---

## projects


---

## links


---

## inspo


---

## ideas


---

## dump


---
```


## 4. Create _index.md in each topic folder

Each one gets a short note describing the folder's scope and companion tags. Keep it to 2-3 lines. Examples:

- Music Knowledge: "composers, pieces, theory, production, listening notes, extended techniques. Tags: #Music, #dailymusic"
- Literature: "books, reading threads, sci-fi, philosophy. Tags: #Literature"
- Hardware: "electronics, modular synth, reverse engineering, SDR, hacking. Tags: #Hardware"
- Design: "print media, typography, visual identity, zines, color theory. Tags: #Design"
- Ai: "model internals, prompting, tools, mech interp, research. Tags: #Ai"
- Video Games: "game design, soundtracks, retro hardware, emulation. Tags: #VideoGames"


## 5. Create empty compiled files

```
_compiled/music.md
_compiled/film.md
_compiled/quotes.md
_compiled/art.md
_compiled/inspiration.md
_compiled/todos.md
_compiled/tobuy.md
_compiled/shopping.md
```

Each one gets a one-line header comment explaining what it is and that it is auto-generated.


## 6. Create all _meta/ protocol docs

This is the big one. Create each of these files with the full protocol content described below.


### _meta/README.md

Session-start checklist and tag conventions. Contents:

The session-start protocol: on every session, grep for pending `#askclaude` tags (excluding `#askclaude-answered`, `#askclaude-deferred`, and `_meta/` itself) and `#link-me` tags (excluding `#link-me-done`). Zero matches = stop, handle whatever the user asks. Matches found = read the relevant protocol doc, process tags, report summary.

Grep pattern for askclaude: `grep -rn --include="*.md" -E '^[[:space:]]*#askclaude([^-]|$)' <vault> | grep -v '/_meta/' | grep -v '/_compiled/'`

Tag conventions table. Two categories:

Tags the assistant acts on:
- `#askclaude` - answer via footnote (protocol: _meta/askclaude/README.md)
- `#link-me` - linking pass, bidirectional wikilinks (protocol: _meta/linking.md)
- `#todo` - compiled to _compiled/todos.md with stable-hash checkboxes (protocol: _meta/todo.md)
- `#inspo` - section captured to _compiled/inspiration.md (protocol: _meta/compile.md)
- `#seed` - creative fragment, compiled per-topic to Topic/seeds.md (protocol: _meta/seed.md)
- `#claudecode` - build idea for Claude Code, creates project brief (protocol: _meta/claudecode.md)
- `#title-me` - fetch page title for a raw URL, replace with titled markdown link (protocol: _meta/title-me.md)
- `#rabbithole` - deep-dive research, creates note in _rabbitholes/ (protocol: _meta/rabbithole.md)
- `#tobuy` - wishlist (unique aspirational items), compiled to _compiled/tobuy.md (protocol: _meta/tobuy.md)
- `#shopping` - recurring purchases, compiled to _compiled/shopping.md weekly rolling (protocol: _meta/shopping.md)
- `#plex` - query Plex library via natural language, returns metadata/posters/lists via footnote (protocol: _meta/plex.md). Requires [plex-usher-mcp](https://github.com/hed0rah/plex-usher-mcp)
- `#code` - programming tool/repo capture. With URL: scrape metadata, file to Programming/{Language}.md. Without URL: treat as idea/todo (protocol: _meta/code.md)
- `#spot` - query Spotify via natural language. Resolve tracks/albums, fetch album art. Primary use: compose with `#dailymusic` to auto-fill Spotify links. Tag disappears on success. Requires [spotify-usher-mcp](https://github.com/hed0rah/spotify-usher-mcp) (protocol: _meta/spot.md)
- `#dailymusic` / `#dailyfilm` / `#dailyquote` / `#dailyart` - line compiled to _compiled/*.md (protocol: _meta/compile.md)

Tags the assistant owns:
- `#claudespeaks` - daily thought, recommendation, question, or idea from the assistant. Written into `## claudespeaks` section of today's daily note. One block per day, no preamble. Protocol: _meta/claudespeaks.md

Tags the assistant never touches:
- `#deepdive` - user marker next to a link that goes deep
- `#followup` - revisit later
- `#Brightidea` - user marker for ideas worth developing
- `#todo-done` - user signal that a todo is closed
- All domain tags (#Music, #Art, #Design, #Hardware, #Literature, etc) are user-only

Footnote prefix table:
- `[^c-N]` - Claude reply
- `[^src-slug]` - source/provenance citation
- `[^YYYY-MM-DD-slug]` - dated future-self annotation
- `[^dig-N]` - digression capture
- `[^def-word]` - glossary/etymology
- `[^note-slug]` - general side note

Folder index listing every folder and its purpose.


### _meta/SYSTEM.md

Full system overview (the shareable version). Cover: philosophy (capture vs structure split), folder layout, daily note conventions (bare date today, topic suffix added next day), tag system, footnote annotation layer, compiled files as caches, promotion workflow (never automatic), linking methodology, inbox, MOCs, weekly digest, scheduled runs (grep-first, early-exit on zero work), trust boundary (user + Obsidian + assistant + filesystem, nothing else).


### _meta/askclaude/README.md

Full protocol for the #askclaude tag:

How the user writes one: single line (`#askclaude question here`) or block form (tag line to next `---`).

How the assistant responds: footnote form. Three cases:
1. Simple case: append `[^c-N]` to the askclaude line, add definition under `## footnotes` section at bottom of file (create if needed), end with ` -- YYYY-MM-DD`
2. Complex case: same marker, but footnote is a one-liner + wikilink to a new topic note where the full answer lives
3. Deferred case: do NOT add marker, add `> claude-deferred: <reason>` below the line, tag stays pristine for re-scan

The original line is never modified beyond the appended footnote marker. No strikethrough, no -answered suffix. The footnote marker IS the answered signal.

Triggers: session start and scheduled daily at 6AM.

Grep pattern: `grep -rn --include="*.md" -E '^[[:space:]]*#askclaude([^-]|$)' /path/to/vault | grep -v '/_meta/'`


### _meta/footnotes.md

Full footnote protocol. Six namespaced prefixes (see table above). Each with purpose, examples, and usage rules.

Claude reply protocol: append `[^c-N]`, definition under `## footnotes`, date-stamped. Old strikethrough pattern stays valid for historical notes, never rewrite.

Provenance footnotes: `[^src-slug]` for factual claims. Compile protocol carries these along.

Dated future-self annotations: `[^YYYY-MM-DD-slug]` for corrections to past notes. Never edit the original line, add a dated footnote instead. The archaeology matters.

Digression capture: `[^dig-N]` for tangents captured without going down the rabbit hole.

Glossary: `[^def-word]` for term definitions.

Section placement: all definitions under `## footnotes` at bottom of file.

Compile interaction: `[^src-]` and `[^def-]` footnotes travel with their lines into compiled files. `[^c-]`, `[^dig-]`, `[^note-]`, and dated footnotes do NOT (marker stays, definition stays in source).

Retrospective grep patterns for each prefix.

What footnotes are NOT for: lists, multi-paragraph explanations, transient todos, structural headers.


### _meta/compile.md

Compiled intake files protocol. Two formats:

Line-based (music, film, quotes, art): `- YYYY-MM-DD: <content> ([[source note]])`. Grep for `#daily<category>`, extract content, write chronologically.

Section-based (inspiration): `## YYYY-MM-DD - [[source]]` sections. Capture from `#inspo` line to next `---` or next tag-prefixed line. Preserve all content verbatim including raw URLs, image embeds, co-occurring tags.

Regeneration: always from scratch, never patch. Compiled files are caches. Daily notes are source of truth.

Footnote interaction: `[^src-]` and `[^def-]` definitions travel with their lines. Others do not.


### _meta/seed.md

Protocol for `#seed` tag. Creative fragments the user generated (not encountered). Counterpart to #inspo. Section-based capture, same rules.

Compile target: per-topic `Topic/seeds.md` (not a single global file like inspo). Routing by co-occurring domain tag.

Format: each seed block gets a `> from [[source daily note]]` header in the collector.

Promotion: seeds graduate to standalone files via normal promotion workflow. Seed entry gets `> developed: [[New File]]` footer.

What is NOT a seed: book recommendations (inspo), todos, reference links (inspo), finished thoughts (topic file directly).


### _meta/claudecode.md

Protocol for `#claudecode` tag. When the user drops a build idea in a daily note, the assistant creates `_projects/<slug>/brief.md` with: what/why (problem and solution), architecture (system design), implementation plan (steps), dependencies (tools, libs, prereqs), open questions, and a ready-to-paste Claude Code session prompt.

The brief is structured so the user can copy it directly into a Claude Code session and start building immediately. Same footnote-marker workflow as `#askclaude`. Processing: generate brief, append `[^c-N]` to the tag line, add footnote with link to the brief.


### _meta/tobuy.md

Protocol for `#tobuy` tag. Wishlist for unique, bigger, one-off items (rare gear, weird objects, curiosities). Compiles to `_compiled/tobuy.md` as a single living list, alphabetical, with stable hash identities (`^tobuy-XXXXXX`) so check state survives regeneration. Items captured from the `#tobuy` line down to the next `---` or next tag-prefixed line. Two completion paths: check box in collector, or add `#tobuy-done` to source line. Image embeds in source stay in source (not duplicated to the list). URLs on continuation lines roll up as parentheticals in the list entry.


### _meta/shopping.md

Protocol for `#shopping` tag. Recurring purchases (groceries, household, consumables). Compiles to `_compiled/shopping.md` as a weekly rolling list with three sections: this week, carried over (unchecked from previous weeks), and completed this week. Monday rollover: unchecked items move to "carried over", checked items archive to `_compiled/shopping/YYYY-Wxx.md`. Stable hashes (`^shop-XXXXXX`) preserve check state. Comma-separated items on one line become separate entries each with their own hash. Co-occurring domain tags preserved as suffix.


### _meta/rabbithole.md

Protocol for `#rabbithole` tag. The assistant creates a thorough standalone research note in `_rabbitholes/` when the user drops this tag with a topic. Output is a well-researched article with sections, sources, and wikilinks back into the vault. If a note on the same topic exists, new findings are appended with a dated section header. Domain tags on the source line are for context only (all rabbitholes route to `_rabbitholes/`). The one place where verbose, link-dense output is encouraged.


### _meta/title-me.md

Protocol for `#title-me` tag. Fetches page title for a raw URL and replaces the line with a titled markdown link (`[Title](url)`). Tag disappears on success. On failure (page unreachable, JS-heavy site, login wall), tag stays for retry and a footnote explains why. One fetch attempt only for hard-to-scrape sites. Never invent titles. Preserves any user text on the line, only the URL gets wrapped.


### _meta/projects.md

Project tracker dashboard protocol. Aggregates projects from multiple signal sources into `_projects/_index.md` with five tiers: active (in development), queued (ready, waiting for bandwidth), stale (no signal in 14+ days), sparks (raw ideas), shipped (completed).

Live signal sources: vault grep (daily note mentions, `#Brightidea` tags), session transcripts (mentions in interactive work), directory scanning of ~/Projects on any machine reachable via SSH, GitHub repo activity.

Each project entry carries `last seen: YYYY-MM-DD` with source tag (vault/session/machine-name/github). Stale detection: projects with zero signal across all sources in 14+ days move to stale tier. Deduplication: same project mentioned across multiple notes/sources rolls up to one tracker entry.

Weekly digest includes project health section reporting tier movements.


### _meta/todo.md

Todo aggregator protocol. `#todo` lines compiled to `_compiled/todos.md`.

Stable identity: `sha1(normalized_text + source_filename)[:6]` as Obsidian block references (`^todo-XXXXXX`). This is how check state survives regeneration.

Two completion paths:
1. Check the box in the collector. Item moves to `## completed` on next regen
2. Add `#todo-done` to the source line. Item disappears entirely on next regen

Collector format: `## active` section grouped by source note (date descending), then `## completed` section.

Regeneration algorithm: grep, compute hashes, read existing collector for checkbox state map, write fresh collector preserving states.

Grep pattern: `grep -rn --include="*.md" -iE '^[[:space:]]*#todo([^-]|$)' /path/to/vault | grep -v '/_meta/' | grep -v '/_compiled/' | grep -v '/_templates/'`


### _meta/linking.md

Wikilink methodology. When to link: direct conceptual dependence, shared thematic core from different angles, shared substantive protagonist, complementary perspectives. When NOT to link: shared tag alone, single keyword overlap, passing mention, same folder.

Two link patterns: inline wikilinks in prose, and a `Related:` section at bottom of note.

Bidirectional is the default. If A links B, B links back to A.

`#link-me` tag: user drops it to request a linking pass. Processing: read note, grep vault for title matches, evaluate candidates, add links, add reverse links, rename tag to `#link-me-done`, log.

The 6AM run does NOT do autonomous vault-wide link passes. Too much mutation.


### _meta/promote.md

Promotion to topic folders. Non-destructive to the daily note. On-demand only, never automatic.

Rules: daily note body stays untouched (only a `> promoted: [[Target]]` footer added). Target note gets the content, reshaped and linked. If target exists, merge into it. If not, create new one with title-case descriptive filename. Always bidirectional wikilinks.

Process: read section, check for existing target, write/edit target, add promoted footer, add Source backlink, run quick linking pass, log.

Voice preservation: do not prose-ify. Raw URLs stay raw, image embeds stay, user tags stay. Add structure around fragments, do not overwrite them.


### _meta/rename.md

Daily note rename methodology. Pattern: `YYYY-MM-DD topic.md`. Topic is 2-4 lowercase words, the single most memorable/unusual thing in the note. Weird specific nouns beat generic categories.

Current day is NEVER renamed (breaks Obsidian's daily note shortcut, topic not knowable yet).

Never rename-and-revert in a single session (virtiofs fuse mount inode cache desync risk; creates permanent 0-byte ghost readable only by session restart).

Before any rename: grep for wikilinks to old name, update them, then rename, then log.


### _meta/digest.md

Weekly digest. Generated Sunday morning at `_digests/YYYY-Wxx.md`. Sections: daily notes this week (one-line summaries), intake counts by category, recurring threads, unresolved askclaude, promotion candidates (listed but NOT auto-promoted), orphan/stub sweep, open #followup tags.

Grep-first: zero daily notes in range = minimal "no activity" digest and exit.


### _meta/moc.md

Maps of Content protocol. Hand-curated hub notes in `_maps/`. Format: intro paragraph, sections of annotated wikilinks, "See also" cross-links to topic folders and related MOCs.

MOCs are never created autonomously. User seeds them. Assistant only adds/removes entries on explicit request.


### _meta/inbox.md

Inbox routing protocol. `_inbox.md` is the capture buffer. Lines route to: today's daily note (default), a topic folder note (if hinted with `-> Folder Name`), an askclaude question (if phrased as a question), append to existing note (`-> append [[Note]]`), or deferred (ambiguous lines stay in inbox with a comment).

Processing: read, split lines, route each, remove routed lines, leave deferred ones. Non-destructive guarantee: no silent drops.


### _meta/voice.md

Writing style observations. Seed it with these starter observations (the user will customize):

Format: daily notes are capture-first, not essay-first. Fragments, bare links, single-line thoughts separated by `---` rules. Tags double as lightweight section headers. Links usually posted raw, not markdown-wrapped.

Voice: low-ego, low-ceremony, high-signal. No hedging. Curiosity across many domains. Comfortable code-switching between deeply technical and aesthetic.

Include a "Living voice protocol" section: the assistant proposes new style observations in a log, surfaces them in interactive sessions for acceptance/rejection. Never silently update the main sections. Never propose more than 3 per session. Never delete log entries.

Include an empty "Observation log" section.


### _meta/decisions.md

Append-only design decisions log. Seed it with one entry:

```
## 2026-xx-xx: initial scaffold

Rule: the full Noggin2 system was scaffolded from a setup prompt.
Reason: bootstrap a complete second-brain with tag-driven LLM processing, footnote annotations, compiled intake files, and topic folder promotion.
Codified in: all _meta/ docs.
```

(Use today's date.)


### _meta/future.md

Deferred ideas. Seed with a few:
- Incremental compilation (only process notes modified since last compile timestamp)
- Per-tag filtering view in the todo aggregator
- Auto-suggest MOC creation when a topic folder exceeds N notes
- Daily note auto-rename on schedule (needs user opt-in)


## 7. Create _meta/askclaude/log/rolling.md

Empty rolling log with a header:

```
# assistant run log

Append-only log of scheduled and interactive processing runs.
```


## 8. Create CHEATSHEET.md at vault root

A concise quick-reference covering: folder layout, all tags (actionable vs user-only), footnote prefixes, key workflows (askclaude, inspo, seed, todo, promote, link-me, inbox, rename), rules, Obsidian plugin setup, and a key files table. This is the "cheat sheet you tape to the wall" version.


## 9. Create .claude/CLAUDE.md

This is the bootstrap prompt that makes future Cowork sessions work automatically. Create it at `.claude/CLAUDE.md` inside the vault folder. Content should include:

- System identity ("You are the assistant for Noggin2")
- Style rules: never use em-dashes, never use emojis, never capitalize first letter in code comments, be concise and technical, creative/free-thinking encouraged, match user voice, prose over bullets for vault content, use footnotes not strikethrough
- Session-start protocol: grep for `#askclaude`, `#link-me`, `#claudecode`, `#title-me`, `#rabbithole`, `#plex`, `#code`, `#spot`. Zero matches = skip to writing `#claudespeaks`. Matches = read protocol docs and process. Always write `#claudespeaks` if section is empty
- Tag vocabulary with protocol doc references for each
- Footnote reply protocol (brief inline version)
- Daily note rules (bare date today, topic suffix next day, append-only, no rename-and-revert)
- Promotion/linking rules (never automatic, user-requested only)
- Compiled files are caches
- Screenshot handling (OCR ok, handwritten stays unless told otherwise)
- Weekly digest (Sunday morning)
- Inbox routing
- Scheduled run behavior (6AM daily, grep-first, always write `#claudespeaks` even on zero-tag days)
- Key reference files list

The CLAUDE.md should reference the _meta/ docs by path so the assistant can lazy-load them. Token conservation: do not read all docs on startup, only when there is actual work.


## 10. After building everything

Tell me:
- What you created (folder count, file count)
- What Obsidian core plugins I need to enable (Daily Notes with template path and date format, Templates, Outline, Graph, Backlinks)
- What to exclude from graph view (_meta/, _compiled/, _digests/, _templates/, _inbox.md)
- That I should customize _meta/voice.md with my own writing style
- That I should customize _templates/daily_template.md with my own intake tags
- That I should add my own topic folders as I go
- Where to check project status: `_projects/_index.md` aggregates all build ideas and work in progress
- That the system is ready to use: just start writing daily notes and dropping tags

Do NOT set up scheduled tasks. I will do that separately once I have used the system interactively for a bit.
