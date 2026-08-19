# Noggin2 cheatsheet

Quick daily reference. Full explanation in `README.md`.

## The idea

Write freely in today's note. Drop tags mid-sentence. The assistant catches up: answers, compiles, carries forward. You never stop to file.

## Daily notes

- Today = `YYYY-MM-DD.md`. Append all day.
- Tomorrow the assistant copies it to `YYYY-MM-DD theme.md` and works in the copy. Delete the bare original once you have glanced at the twin.
- Append-only. Corrections are dated footnotes, never edits.

## 1. Inline tags (acted on automatically)

| tag | what happens |
| --- | --- |
| `#claude` | answers your question as a footnote `[^c-N]`, your line untouched |
| `#todo` | added to the rolling checklist (stable-hash checkboxes survive rebuilds) |
| `#tobuy` | added to the wishlist |
| `#reminder` | resurfaced later in `## recall`, rotated by relevance |

## 2. Compiled intake (just filed)

`#dailymusic` `#dailyfilm` `#dailyquote` `#dailyart` `#dailymeme` `#dailygame` -> each line to `_compiled/<name>.md`. Add your own daily-* tag anytime.
`#inspo` -> inspiration you grabbed, to `_compiled/inspiration.md`.
`#seed` -> an idea that is yours, to `<Topic>/seeds.md`. (grabbed = inspo, made = seed.)

## 3. `#run <verb>` (on-demand, occasional)

| command | does |
| --- | --- |
| `#run deepdive <topic>` | structured research note |
| `#run rabbithole <topic>` | verbose exploration note in `_rabbitholes/` |
| `#run link [note]` | bidirectional linking pass |
| `#run title <url>` | wrap a bare URL as `[Title](url)` |
| `#run promote <what> -> <Folder>` | move daily content into a topic folder |
| `#run project <idea>` | scaffold `_projects/<slug>/brief.md` |
| `#run review <thing>` | critique a note/plan/draft: gaps, errors, weak spots |
| `#run digest` | build this week's digest |
| `#run rebuild <cache>` | regenerate a compiled file |

Bare `#deepdive` / `#rabbithole` = quiet "maybe later" markers. `#run deepdive <topic>` = do it now.

## Tags it never touches (your search markers)

`#followup` and all domain tags (`#music`, `#hardware`, ...).

## Sections the assistant writes

- `## claudespeaks` - one daily thought from your recent activity.
- `## recall` - a couple of reminders surfaced when relevant.
- `## CONTINUED` - yesterday's open threads, seeded automatically, terse with footnotes.

## Footnotes (greppable history)

`[^c-N]` answers, `[^cf-N]` carried-forward, `[^remind-N]` reminders. All end ` -- YYYY-MM-DD`.
`grep -rn '\[^c-' .` = every answer you ever got.

## Inbox

One line in `_inbox.md`, routed next pass: default -> today, `-> Folder`, `-> append [[Note]]`, `-> claude`, `-> defer`.

## When in doubt

Just write. Tag if you feel like it. The system catches up.
