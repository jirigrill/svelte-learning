# Mission: Svelte 5 + JavaScript (just enough) to own `atopic_helper`

## Why
Jiri is the author of `atopic_helper` (a Svelte 5 + SvelteKit + IndexedDB PWA, located at `../atopic_helper/`) and wants to read, debug, and confidently extend it without leaning on AI for every line. His main programming languages are Python and a little Rust; the JS/TS stack is the unfamiliar part. Success is measured by the ability to make a deliberate change in `atopic_helper` end-to-end and explain *why* it works.

## Success looks like
- Can read any `.svelte` file in `atopic_helper` and explain what each rune (`$state`, `$derived`, `$effect`, `$props`) is doing and when it runs.
- Can add a new route, page, and component to `atopic_helper` from scratch — wiring it into a layout, passing props, and persisting data to Dexie/IndexedDB.
- Can debug reactivity bugs ("why didn't this update?") by reasoning about Svelte's signal-based runtime, not by guessing.
- Comfortable enough with JS/TS basics — modules, `const`/`let`, arrow functions, destructuring, `async`/`await`, optional chaining, template literals, array methods, narrow type annotations — that nothing in `atopic_helper`'s code looks like noise.
- Can add a Vitest unit test for a piece of domain logic in `src/lib/`.

## Constraints
- Real project to ground every lesson: `../atopic_helper/` (Svelte 5.55, SvelteKit 2.60, `adapter-static`, Dexie, Tailwind v4, TypeScript, Vitest, Playwright, PWA).
- Learner background: strong Python, light Rust, some HTML/CSS, no prior JS framework experience.
- "Just enough JS" — teach JS/TS *as it shows up* in the codebase, not as a separate curriculum. When a JS concept is the blocker (e.g. closures, `this`, async), break out a focused lesson.
- Lessons should be short and end with a tangible win (a runnable change, a question answered about real code).

## Out of scope
- Server-rendered SvelteKit features (server `+page.server.ts` load functions, form actions, hooks for sessions/cookies). The app is a static-export PWA — no Node server in production. We will note these exist but not teach them deeply unless the mission shifts.
- Older Svelte 3/4 reactivity (`$:`, stores-as-default, `export let`). Skim only when reading legacy docs or other people's tutorials.
- Build-tool deep dives (Vite internals, bundler theory) beyond what's needed to be unblocked.
- Backend frameworks, Node.js server programming, deployment beyond the static-PWA path the project already uses.
