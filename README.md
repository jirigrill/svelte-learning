# svelte-learning

A structured learning workspace for Svelte 5 + SvelteKit, grounded in a real project ([atopic_helper](https://github.com/jirigrill/atopic_helper)).

Lessons progress through the Svelte 5 runes system — `$state`, `$derived`, `$effect`, props, components, SvelteKit routing — with an interactive playground app and printable reference sheets.

## Structure

```
lessons/          HTML explainer for each lesson (open in browser to read)
playground/       Runnable SvelteKit app — one route per lesson exercise
reference/        Printable cheat sheets (populated as lessons progress)
learning-records/ What has been learned and verified, in ADR style
MISSION.md        Why this workspace exists and what success looks like
RESOURCES.md      Curated sources: official docs, communities
NOTES.md          Teaching preferences and mental-model notes
```

## Lessons so far

| # | Topic | Explainer | Exercise |
|---|-------|-----------|---------|
| 01 | `$state` — reactive variables | `lessons/0001-state-rune.html` | `/lesson/state` |
| 02 | `$derived` — computed values | `lessons/0002-derived-rune.html` | `/lesson/derived` |

## Running the playground

```bash
cd playground
bun install      # first time only
bun run dev      # starts at http://localhost:5180
```

Open the lesson index at `http://localhost:5180` and pick a lesson, or navigate directly (e.g. `http://localhost:5180/lesson/derived`).

To open a lesson explainer:

```bash
open lessons/0001-state-rune.html
open lessons/0002-derived-rune.html
```

## How this workspace is used with Claude Code

This repo is a teaching workspace for the [`/teach` skill](https://github.com/spences10/svelte-skills) in [Claude Code](https://claude.ai/code).

### Starting a new session

Open Claude Code in this directory and run:

```
/teach svelte language together with javascript basics
```

Claude will read `MISSION.md`, `NOTES.md`, and `learning-records/` to pick up exactly where you left off — no re-introduction needed.

### How `/teach` works

- **`MISSION.md`** anchors every lesson to a concrete goal. Claude won't teach abstract theory; it teaches what you need to make progress on your actual project.
- **`learning-records/`** are the memory of the workspace. Claude reads them to calculate your zone of proximal development — what to teach next, what not to repeat.
- **`NOTES.md`** stores teaching preferences (e.g. "never ask me to edit the real project directly — use the playground").
- **Lessons** are HTML files you keep. They are designed to be re-read months later and still make sense.
- **The playground** is a throwaway SvelteKit app. Exercises live there so the real project (`atopic_helper`) is never modified for learning purposes.

### Resuming

Each new session, Claude reads the workspace state automatically. To request the next lesson just say:

```
lesson 3
```

or ask about something you encountered in the real codebase:

```
I see $effect in day-view.svelte.ts — what does it do?
```

Claude will teach it in context and write the appropriate learning record and lesson file.
