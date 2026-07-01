# Lesson 1: $state understood, with two corrections

Jiri completed Lesson 1 (the playground exercise + self-check questions) on 2026-06-14. He has a solid working model of `$state` as "reactive variables that re-render the UI on write," and correctly explained that arrays/objects under `$state` are reactive on in-place mutations (Proxy-backed deep reactivity).

Two corrections logged from the self-check; both are recurring Python/Rust → JS traps worth carrying into future lessons:

**Correction 1 — `const` is binding-immutable, not value-immutable.**
Jiri initially said `const { value } = obj` made `value` non-updating "because it's a const." The actual reason is that **destructuring takes a snapshot copy** — the new local variable no longer reads through the reactive Proxy. `const` only forbids re-binding the name; the data behind it is still mutable. Tag any future lesson involving destructuring, prop-passing, or store access with this caveat.

**Correction 2 — `$state` is for values that change over time AND drive the UI.**
Jiri's answer to "why isn't `TOTAL_STEPS` wrapped in `$state`?" described what the values mean rather than the technical rule. He understood the distinction once corrected. The rule itself — *don't wrap constants* — is now established and should not be re-taught.

## Implications
- Lesson on **props / `$bindable`** must explicitly cover the destructuring trap (it's the #1 way reactivity silently breaks at component boundaries).
- Lesson on **stores / context** should re-establish "read through the source, not through a copy" using the same mental model.
- The "what counts as state vs. constant" rule is in place; Lesson 2 (`$derived`) can build on it without re-deriving.

## Follow-up: deeper understanding established (same session)
After the bonus exercise, Jiri spotted *unprompted* that the demo design implies "the value `'Jiri'` is duplicated" and asked whether that violates DRY. Walking through it together established that the literal `'Jiri'` is in the source only once (at the `$state(...)` initialiser), but at *runtime* there are two separate string values in memory after destructuring — that's exactly why they can diverge. He intuited the **single-source-of-truth** principle from the destructuring trap without having seen it named. This is decision-grade signal: he's reasoning about reactivity in terms of memory-level state mirroring, not just framework-level "does it update?" mechanics. Future lessons can lean on this as established vocabulary.

He then *also* spotted that the bonus's click handler had `'Jiri'` hardcoded in two places (literal duplication, not just runtime duplication) — a real DRY violation, fixed by extracting `INITIAL_NAME`. Reading code for duplicate values is already an instinct.

He then *also* asked whether `const { name } = user` is "useless" given there's no functional reason to keep it. This required teaching the distinction between **production code** (every line earns its keep) and **demo/test code** (some lines exist to expose a phenomenon). Both forms of "useless" question — the literal-duplication one and the demo-line one — show he's reading code with an editor's eye, not a consumer's. Lessons should now assume he will challenge unjustified lines and explain *why* a line exists, not just *what* it does.
