# Noggin2

A personal second-brain system built on Obsidian + a local LLM assistant (Claude in cowork mode). No community plugins. No cloud ingest. Everything is plain markdown in one folder. Designed to survive ten years of daily use without ever requiring a migration.

The core idea: capture fast, process later. Daily notes are the raw stream. Topic folders are the refined knowledge base. An LLM bridges the two using tags as the interface. You never have to stop and think about where something goes. Just write. The system catches up.

---

## Folder layout

```
Notez/
  _inbox.md                 friction-free capture from anywhere
  _meta/                    protocol docs, assistant memory, decisions log
  _templates/               daily note template
  _compiled/                auto-generated intake logs and todo list
  _digests/                 weekly digests
  _maps/                    Maps of Content (hand-curated hubs)
  _projects/                project tracker and Claude Code briefs
  _rabbitholes/             deep-dive research notes
  YYYY-MM-DD topic.md       daily notes at the vault root
  Music Knowledge/          topic folder (playlists, theory, instruments)
  Movies/                   topic folder (letterboxd, anime, plex exports)
  Skateboarding/            topic folder (creative skaters, off-road, playlists)
  Design/                   topic folder (typography, logo, print and layout)
  Sculpture_Architecture/   topic folder
  Literature/               topic folder
  Hardware/                 topic folder
  Ai/                       topic folder
  Video Games/              topic folder
  Programming/              topic folder (tools/repos by language, auto-filed by #code)
  Cartography_Maps/         topic folder
```

Hide from graph view: `_meta/`, `_compiled/`, `_digests/`, `_templates/`, `_rabbitholes/`, `_inbox.md`

---

## Daily notes

One per day. Today is always bare `YYYY-MM-DD.md`. After the day rolls over, Claude adds a 2-4 word topic suffix: `2026-04-11 ripleys seiko.md`. The topic is the single most memorable or unusual thing in the note, not the most important. This gives graph view actual memory hooks instead of a wall of dates.

Template at `_templates/daily_template.md` gets auto-filled by Obsidian's core Daily Notes plugin. Sections: daily intake tags at top, then `## claudespeaks` (Claude's daily thought), `## projects` (active build work), `## links` (repos, articles, tools, references), `## inspo` (visual/design inspiration, images), `## ideas` (brightideas, seeds, askclaude questions, speculative stuff), `## dump` (everything else). Sections are guidance, not walls. If you're in a hurry, throw everything in dump.

Daily notes are append-only. Claude never rewrites the body. Its edits are limited to appending footnote markers (`[^c-1]`) and definitions in a `## footnotes` section at the bottom, and writing the `#claudespeaks` block during scheduled run.

---

## Tags

### Tags Claude acts on

| Tag | What happens |
|---|---|
| `#askclaude` | Claude answers via footnote. Simple answers go in `## footnotes` at bottom of note. Complex answers get their own topic-folder note, referenced from a short footnote |
| `#link-me` | Claude does a linking pass: finds related notes, adds wikilinks, makes them bidirectional |
| `#todo` | Compiled into `_compiled/todos.md` as a checkbox list. Check the box to mark done. Add `#todo-done` to the source line to archive it out of the list entirely |
| `#inspo` | Section captured into `_compiled/inspiration.md`. Drop alongside a domain tag (`#Design`, `#Hardware`, etc) for filtering. Captures from the tag line down to the next `---` or next tag-prefixed line |
| `#seed` | Your own creative fragments (story ideas, premises, sketches). Compiled per-topic into `Topic/seeds.md`. Same section capture as `#inspo` but routes by co-occurring domain tag |
| `#claudecode` | Idea for something to build in Claude Code. Claude creates a project brief at `_projects/<slug>/brief.md` with architecture, implementation plan, and a ready-to-paste Claude Code session prompt |
| `#title-me` | Fetch page title for a raw URL and replace with a titled markdown link. Tag disappears on success. On failure, tag stays and a footnote explains why |
| `#rabbithole` | Deep-dive research. Claude creates a thorough standalone note in `_rabbitholes/` with structured research, sources, and wikilinks back into the vault |
| `#tobuy` | Wishlist item (unique, bigger, one-off). Compiled to `_compiled/tobuy.md` as a living list. Check the box when acquired, or add `#tobuy-done` to archive |
| `#shopping` | Recurring shopping (groceries, consumables). Compiled to `_compiled/shopping.md`. Check the box when bought, or add `#shopping-done` to archive |
| `#plex` | Query your Plex library via natural language. Claude searches, pulls metadata, downloads posters, exports lists. Can enrich with RT/IMDB/Metacritic scores via usher-tools MCP. Results go in footnotes. Requires [plex-usher-mcp](https://github.com/hed0rah/plex-usher-mcp) |
| `#code` | Programming tool/repo capture. With a GitHub/PyPI URL: scrape metadata (name, language, description), file to `Programming/{Language}.md`, title the link. Without URL: treat as code idea/todo |
| `#spot` | Query Spotify via natural language. Resolve tracks/albums, fetch album art, pull metadata. Primary use: compose with `#dailymusic` to auto-fill Spotify links. Tag disappears on success. Requires [spotify-usher-mcp](https://github.com/hed0rah/spotify-usher-mcp) |
| `#dailymusic` | Line compiled into `_compiled/music.md` |
| `#dailyfilm` | Line compiled into `_compiled/film.md` |
| `#dailyquote` | Line compiled into `_compiled/quotes.md` |
| `#dailyart` | Line compiled into `_compiled/art.md` |
| `#dailymeme` | Line compiled into `_compiled/memes.md` |
| `#dailygame` | Line compiled into `_compiled/games.md` |

### Tags Claude owns

| Tag | What it is |
|---|---|
| `#claudespeaks` | Claude's daily thought in `## claudespeaks`. Music recs, ideas, questions, connections to recent vault activity. One block per day, written during scheduled run or session start |

### Tags Claude never touches

| Tag | What it is |
|---|---|
| `#deepdive` | Your marker next to a link that goes deep. Not a request to Claude |
| `#followup` | Revisit later. Same deal |
| `#Brightidea` | Your marker for ideas worth developing |
| `#todo-done` | Your signal that a todo is closed. Claude filters it out on next regen |

### Your domain tags (unchanged)

`#Music` `#Quote` `#Movie` `#Literature` `#Art` `#Design` `#Audio-synth` `#Hardware` `#Brightidea` `#Biology` `#Nanotechnology` `#Ai` `#Reverseengineering` `#GPU` `#mechs` `#space` `#arg` `#vhs` `#Robotics` `#philosophy` `#Burndownthestack` `#HoL` `#science` `#cartography`

Claude reads these for context and co-occurrence but never processes them independently. They are yours.

---

## Footnotes

Footnotes are the annotation layer. They keep the main body pristine while carrying commentary, sources, corrections, and Claude's replies underneath. Obsidian renders them natively (Ctrl-click to jump to definition).

Every footnote marker uses a namespaced prefix:

| Prefix | Purpose | Example |
|---|---|---|
| `[^c-N]` | Claude's reply to `#askclaude` | `[^c-1]` |
| `[^src-slug]` | Source citation | `[^src-wikipedia]` |
| `[^YYYY-MM-DD-slug]` | Future-you annotating past-you | `[^2026-04-12-wrong]` |
| `[^dig-N]` | Digression you pulled out of | `[^dig-1]` |
| `[^def-word]` | Glossary / etymology drop | `[^def-qid]` |
| `[^note-slug]` | General side note | `[^note-fuzz-pedals]` |

Definitions live under a `## footnotes` section at the bottom of the note. Claude always creates this section when adding its first footnote to a file.

The rule: never edit a past line to match new information. If something is wrong, add a dated footnote. The archaeology matters.

---

## Workflows

### Ask Claude a question

```
#askclaude what is the name of that Raymond Scott sequencer with rotating contacts
```

Claude appends `[^c-1]` to the line and puts the answer in `## footnotes`:

```
#askclaude what is the name of that Raymond Scott sequencer[^c-1]

## footnotes

[^c-1]: the Circle Machine, built ~1959 as part of the Electronium project. -- 2026-04-12
```

If the answer is big enough for its own note, the footnote is a one-liner with a wikilink: `[^c-1]: full list in [[Experimental Sculpture]]. -- 2026-04-12`

### Drop inspiration

```
#inspo #Design tunnel books / accordion style 3d https://rarebooks.uflib.ufl.edu/...
https://x.com/historichub/status/...
```

The section from `#inspo` to the next `---` gets captured into `_compiled/inspiration.md`. Filter later with Obsidian search: `path:_compiled/inspiration tag:#Design`.

### Drop a creative seed

```
#seed #Literature A snail baby lives its life, growing older and longer,
climbing the ever lengthening staircase that is its shell.
```

Compiled into `Literature/seeds.md`. Seeds are things you generated. Inspo is things you grabbed. They compile separately because they want different treatment: inspo gets returned-to-and-consumed, seeds get returned-to-and-developed.

### Capture a todo

```
#todo build a compiler that mimics the genome
```

Shows up in `_compiled/todos.md` as:

```
- [ ] build a compiler that mimics the genome  ^todo-27840b
```

Check the box when done. Or add `#todo-done` to the line in the daily note to archive it completely.

### Title a raw URL

```
#title-me https://www.luhringaugustine.com/exhibitions/emily-kraus
```

Becomes: `[Emily Kraus - Luhring Augustine](https://www.luhringaugustine.com/exhibitions/emily-kraus)`. Works with surrounding text too. If the page can't be fetched, tag stays and you get a footnote explaining why.

### Capture something to buy

```
#tobuy Chinese Pickling Jar: Water Moat Seal
#shopping olive oil, coffee filters
```

`#tobuy` for wishlist (unique, rare, aspirational). `#shopping` for recurring purchases. Both compile to separate living lists with stable checkbox state. Check the box when acquired, or add `-done` suffix to the source tag.

### Go down a rabbithole

```
#rabbithole #Hardware how CRT electron guns actually work
```

Claude creates `_rabbitholes/crt-electron-guns.md` with a thorough research piece: history, technical details, sources, wikilinks to related vault notes. The one place where verbose output is the point.

### Kick off a Claude Code project

```
#claudecode build an MCP server that wraps the discogs API
```

Claude creates `_projects/discogs-mcp/brief.md` with: what/why, architecture, implementation steps, dependencies, open questions, and a ready-to-paste Claude Code session prompt. Edit the brief as the project evolves.

### Capture a code tool or repo

```
#code https://github.com/dmtrKovalenko/fff.nvim
#code #Hardware ESP32 bus pirate - https://github.com/geo-tp/ESP32-Bus-Pirate
```

Claude scrapes the repo metadata (name, description, primary language), replaces the line with a titled link (tag disappears), and appends the entry to `Programming/{Language}.md`. Without a URL, the tag just marks a code idea. 60 repos already filed across 12 languages from the initial vault sweep.

### Auto-fill a daily music entry from Spotify

```
#dailymusic Tim Exile - #spot what album was I listening to earlier
#dailymusic #spot Hedex & Ray Volpe - new collab. pull album art and link
```

Claude resolves the query via Spotify (search, recently played, artist discography), rewrites the line with a titled Spotify link, saves album art to the vault if requested, and removes the `#spot` tag. The `#dailymusic` tag stays for compilation. Requires [spotify-usher-mcp](https://github.com/hed0rah/spotify-usher-mcp).

### Query your Plex library

```
#plex what actors were in The Thing?
#plex pull the movie poster for Akira
#plex list my unwatched anime
#plex export my full horror collection
```

Claude searches your Plex library (fuzzy matching on titles), pulls metadata or poster images, and writes results into a footnote. For image requests, the poster gets saved to the vault and embedded with `![[plex-akira-1988-poster.jpg]]`. Large exports generate a separate file in `Movies/`. Requires [plex-usher-mcp](https://github.com/hed0rah/plex-usher-mcp) running on the same LAN as your Plex server.

### Check your project dashboard

Open `_projects/_index.md`. Five tiers: active (building now), queued (ready to start), sparks (raw ideas), shipped. Auto-compiled from `#Brightidea`, `#claudecode`, and `#projects` tags. Tier placement is manual but the assistant suggests promotions during weekly digest.

### Promote daily-note content to a topic folder

Ask Claude explicitly: "promote the seiko stuff from today into Design/". Claude creates or appends to a note in the target folder, adds a `Source: [[daily-note]]` header in the target, and adds a small `> promoted: [[Target]]` footer in the daily note. The daily note body is never rewritten.

Promotion is never automatic. Always on your request.

### Request a linking pass

```
#link-me
```

Drop it in a note. Claude finds related notes via grep, adds wikilinks inline and/or a `Related:` footer, and makes them bidirectional (if A links to B, B gets a link back to A).

### Quick capture from anywhere

Open `_inbox.md`. Type one line. Save. Claude empties it next session, routing by hints:

- no hint or `-> daily` goes into today's daily note
- `-> Music Knowledge` goes into that topic folder
- `-> append [[Note Name]]` appends to an existing note
- `-> askclaude` treated as a question
- `-> defer` stays in inbox

### Regenerate compiled files

Ask "rebuild the compiled files" or "update todos.md". Claude regenerates from scratch by grepping daily notes. Compiled files are caches. The daily notes are always the source of truth.

### Weekly digest

Auto-generated Sunday morning at `_digests/YYYY-Wxx.md`. Summary of the week's daily notes, intake counts by category, recurring themes, unresolved askclaude, promotion candidates, and orphan notes.

---

## The inspo/seed split

Two tags for two kinds of "things worth coming back to":

`#inspo` is external. A link, an image, a book recommendation, a video, a design reference. Origin is outside you. Compiles into one global file: `_compiled/inspiration.md`.

`#seed` is internal. A story fragment, a premise, a character sketch, a worldbuilding kernel. Origin is you. Compiles per-topic: `Literature/seeds.md`, `Hardware/seeds.md`, etc.

Both use section capture (tag line to next `---`) and both use co-occurring domain tags for filtering. The split exists because scrolling past your own ideas thinking they are someone else's links is a failure mode.

---

## The todo aggregator

`_compiled/todos.md` is a living checkbox list compiled from every `#todo` line in the vault. Each item has a stable Obsidian block reference (`^todo-XXXXXX`) derived from a hash of the text and source, so your check state survives when the list is regenerated.

Two ways to mark done:

1. Check the box in the collector. Item moves to `## completed` on next regen.
2. Add `#todo-done` to the line in the daily note. Item disappears from the collector entirely.

---

## Obsidian setup

All core plugins, zero community plugins.

**Daily Notes plugin:** template path `_templates/daily_template`, date format `YYYY-MM-DD`, new file location at vault root.

**Templates plugin:** for manual template insertion via hotkey.

**Outline, Graph, Backlinks:** core navigation.

**Excluded from graph:** `_meta/`, `_compiled/`, `_digests/`, `_templates/`, `_rabbitholes/`, `_inbox.md`

**Obsidian Sync** (optional, paid): phone to desktop replication. The system works without it but Sync is how phone captures reach the desktop vault.

---

## Rules

1. Today's daily note is never renamed. Topic suffix gets added the next day or later.
2. Daily notes are never rewritten. Claude's edits are surgical (footnotes, `#claudespeaks`, and `> promoted:` footers only).
3. Promotion to topic folders is always user-requested, never automatic.
4. Compiled files are caches. Regenerate from scratch. Never hand-edit (except checkbox state in todos).
5. `#daily*` tags are paired with base tags, not replacing them. Both stay.
6. Scheduled runs are grep-first: zero work means one grep and one log line. No wasted tokens.
7. The assistant never does rename-and-revert on a file in the same session (virtiofs cache desync risk on fuse mounts).
8. Screenshots can be OCR'd and removed. Handwritten content stays unless explicitly told otherwise.
9. Never edit a past line to correct it. Add a dated footnote instead.
10. No community plugins, no cloud LLM ingest, no automation platforms. Obsidian core + Sync + local assistant. That is the trust boundary.

---

## How to replicate this

1. Install Obsidian, create a vault
2. Create the folders: `_meta/`, `_meta/askclaude/`, `_meta/askclaude/log/`, `_templates/`, `_compiled/`, `_digests/`, `_maps/`, plus topic folders
3. Copy the `_meta/` protocol docs as starting points. Adapt `voice.md` to your own style
4. Enable core plugins: Daily Notes, Templates, Outline, Graph, Backlinks
5. Set the graph exclusions
6. Set up a cowork session (or any local LLM agent) pointed at the vault folder with read/write access
7. Configure a daily 6AM scheduled task: grep for pending tags, process `_inbox.md`, process tags, log results
8. Configure a Sunday morning digest task
9. Start capturing. Drop tags wherever. Let the system catch up

---

## Key files

| File | What it is |
|---|---|
| `_meta/SYSTEM.md` | Full system doc, shareable |
| `_meta/README.md` | Session-start checklist, tag conventions |
| `_meta/decisions.md` | Why the rules are the way they are |
| `_meta/footnotes.md` | Footnote annotation protocol |
| `_meta/seed.md` | Seed tag protocol |
| `_meta/todo.md` | Todo aggregator protocol |
| `_meta/askclaude/README.md` | Askclaude tag protocol |
| `_meta/voice.md` | How you write, so Claude can match tone |
| `_meta/rename.md` | Daily note filename convention |
| `_meta/compile.md` | Intake compilation rules |
| `_meta/linking.md` | Wikilink methodology |
| `_meta/promote.md` | Topic-folder promotion workflow |
| `_meta/digest.md` | Weekly digest generation |
| `_meta/moc.md` | Maps of Content protocol |
| `_meta/inbox.md` | Inbox routing protocol |
| `_meta/claudecode.md` | Claude Code project brief protocol |
| `_meta/title-me.md` | URL title fetching protocol |
| `_meta/rabbithole.md` | Deep-dive research protocol |
| `_meta/tobuy.md` | Wishlist compile protocol |
| `_meta/shopping.md` | Shopping list compile protocol |
| `_meta/plex.md` | Plex library query protocol |
| `_meta/code.md` | Code/repo capture and filing protocol |
| `_meta/claudespeaks.md` | Daily thought protocol |
| `_meta/projects.md` | Project index compile protocol |
| `_projects/_index.md` | Your project dashboard (compiled) |
| `_rabbitholes/` | Deep-dive research notes |
| `_meta/future.md` | Deferred ideas, not yet built |
| `_meta/tag-incubator.md` | Living log of unrecognized tags, candidates for promotion |

---

## When in doubt

Just capture. Drop it in a daily note or `_inbox.md` with whatever tags feel right. File-friction is the enemy of a second brain. You and Sunday-you will sort it out later.
