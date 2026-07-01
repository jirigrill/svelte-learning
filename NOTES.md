# Working Notes

## Learner context
- Author of `../atopic_helper/` — uses every lesson should reference real code from there when possible.
- Stack: Python (primary), Rust (light), HTML/CSS (some). No prior JS framework experience.
- Email: jiri.grill@workday.com.

## Teaching preferences
- "Just enough JS" — JS/TS taught only as needed to unblock Svelte work.
- Codebase is TypeScript; lessons should show TS where the project uses it, but always explain the underlying JS concept first so TS is just "JS + annotations" in the learner's head.
- Lessons should be short, end with a runnable win, and ideally include a question about the actual `atopic_helper` source.
- **NEVER ask the learner to modify `atopic_helper` directly.** Instead, copy the relevant snippets/files into this `svelte_learning/` workspace under `playground/` and let exercises run there. `atopic_helper` is read-only reference material — extract from it, don't poke it.
- Each lesson that has a hands-on task should ship a self-contained `playground/NNNN-name/` mini-app (or single file) the learner can run in isolation. Provide the run command in the lesson.

## Mental-model bridges to lean on
- Python → JS: explicit `let`/`const`, no indentation-as-syntax, arrow functions ≈ lambdas, destructuring ≈ tuple unpacking, `async`/`await` works the same, `Promise` ≈ `Future`/`Task`.
- Rust → TS: types are checked at compile only (erased at runtime, unlike Rust), no ownership, no borrowing — but generics, sum types via unions (`'a' | 'b'`), and `Result`-like error handling feel familiar.
- HTML → Svelte: a `.svelte` file *is* HTML, with a `<script>` block on top and reactive `{expressions}` in the markup.

## Pacing signal
- After each lesson, ask whether it was too easy / about right / too hard. Adjust depth in the next lesson accordingly.
- If user says "I already knew that" — write a learning record so we don't re-cover it.

## Open questions to revisit
- Does Jiri want me to author lessons in Czech occasionally? (codebase has Czech strings — `commonStrings`, `allergenWordCs`)
- How comfortable is he with the terminal / git / running `bun dev`? (likely fine, but check before assuming)
