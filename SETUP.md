# Noggin2 setup prompt

Paste this to your assistant once, inside your new vault, to scaffold the system. It assumes `CLAUDE.md` is at `.claude/CLAUDE.md` and `_templates/daily_template.md` exists.

---

You are setting up a Noggin2 vault. Create the following, then stop and report what you made. Do not process anything yet.

## 1. Folders and buffers

`_meta/`, `_compiled/`, `_templates/` (exists), and `_inbox.md` at the root (a friction-free capture buffer, one line per idea).

## 2. Empty compiled caches in `_compiled/`

Empty files with a one-line "auto-generated cache" header each: `todos.md`, `tobuy.md`, `reminders.md`, `inspiration.md`, and one per daily-intake tag (`music.md`, `film.md`, `quotes.md`, `art.md`, `memes.md`, `games.md`).

## 3. Protocol docs in `_meta/`

One short doc per mechanism. Keep each tight; they load only when there is work.

Inline tags:
- `askclaude.md` - answering `#claude` as a `[^c-N]` footnote without editing the user's line.
- `todo.md` - `#todo` compiling into the checklist, with stable `^todo-XXXXXX` block refs so check-state survives rebuilds.
- `tobuy.md` - `#tobuy` living wishlist, same stable-hash checkboxes.
- `reminder.md` - `#reminder` compiling, and `## recall` rotating 2 to 3 items by relevance.

Compiled intake:
- `compile.md` - the daily-* tags copying into `_compiled/<name>.md`, and `#inspo` capturing to `_compiled/inspiration.md`.
- `seed.md` - `#seed` capturing the user's own idea-fragments to `<Topic>/seeds.md`, and why it is split from inspo.

The `#run` dispatcher and its verbs:
- `run.md` - the verb list and dispatch rule.
- `deepdive.md` - `#run deepdive` structured research note in house style.
- `rabbithole.md` - `#run rabbithole` verbose note in `_rabbitholes/`, append-if-exists.
- `linking.md` - `#run link` bidirectional linking pass.
- `title.md` - `#run title` fetch-and-wrap a bare URL.
- `promote.md` - `#run promote` moving daily content to a topic folder with backlinks both ways.
- `project.md` - `#run project` scaffolding `_projects/<slug>/brief.md`.
- `digest.md` - the weekly digest (`#run digest` or Sunday).

System:
- `rename.md` - copy-to-themed-twin and the frozen-original rule.
- `footnotes.md` - the prefixes (`[^c-N]`, `[^cf-N]`, `[^remind-N]`) and the ` -- YYYY-MM-DD` suffix.
- `claudespeaks.md` - writing the one daily assistant thought so it is specific, not filler.
- `inbox.md` - `_inbox.md` routing hints.
- `tag-incubator.md` - a living log of unrecognized tags, with a promotion path.
- `decisions.md` - an append-only log of why the rules are what they are.

## 4. Assistant memory

`_meta/askclaude/log/rolling.md` - your run log, one short entry per catch-up pass.

## 5. Confirm

List everything you created and the current tag vocabulary (inline, compiled, `#run` verbs). Do not process anything yet.
