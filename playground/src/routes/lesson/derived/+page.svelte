<script lang="ts">
  // ════════════════════════════════════════════════════════════════
  // Lesson 0002 · $derived — computed values that recompute themselves
  // ════════════════════════════════════════════════════════════════
  // A miniature questionnaire mock — same pattern as
  // atopic_helper/src/routes/+page.svelte:
  //
  //     let step = $state(1);
  //     const progress = $derived(((step - 1) / (TOTAL_STEPS - 1)) * 100);
  //
  // Click "next"/"back". Notice what's NOT in the click handler:
  // any code recomputing `progress`, `stepLabel`, `status`, etc.
  // The runes do that for free.

  const TOTAL_STEPS = 6;
  const stepNames = [
    'Birth date',
    'Severity',
    'Mother allergens',
    'Baby allergens',
    'Start date',
    'Confirm'
  ];

  // ─── Inputs (state) ───────────────────────────────────────────
  let step = $state(1);

  // ─── Single-expression derived: the workhorse form ───────────
  // Recomputes whenever `step` changes. Read-only (note `const`).
  const progress = $derived(((step - 1) / (TOTAL_STEPS - 1)) * 100);

  // The label depends on `step` and the const array `stepNames`.
  // Only `step` is reactive — the array is captured at module init.
  const stepLabel = $derived(stepNames[step - 1]);

  // ─── Multi-line derived: $derived.by() form ──────────────────
  // Use this when the formula doesn't fit nicely on one line, or
  // when you want intermediate names for readability. The function
  // body runs every time a dependency changes (same as the
  // single-expression form).
  const status = $derived.by(() => {
    const ratio = step / TOTAL_STEPS;
    if (ratio < 0.34) return { tone: 'cool', text: 'Just getting started' };
    if (ratio < 0.67) return { tone: 'warm', text: 'Halfway there' };
    if (ratio < 1) return { tone: 'hot', text: 'Almost done' };
    return { tone: 'done', text: 'Complete!' };
  });

  // ─── Multiple inputs, one output ──────────────────────────────
  // Try changing TOTAL_STEPS at the top to see how the formula
  // adapts — this expression has only ONE reactive dependency
  // (`step`) but reads two things; the const value is baked in.
  const stepsRemaining = $derived(TOTAL_STEPS - step);

  // ─── Click handlers — note how SHORT they are ───────────────
  function next() {
    if (step < TOTAL_STEPS) step++;
    // No `progress = ...` here. No `stepLabel = ...` here. Svelte
    // figures out what changed and re-derives everything that
    // depends on `step`.
  }
  function back() {
    if (step > 1) step--;
  }
  function reset() {
    step = 1;
  }
</script>

<main>
  <p class="back"><a href="/">← lesson index</a></p>
  <h1>Lesson 2 · <code>$derived</code></h1>
  <p class="lede">
    Click "next" / "back". Watch the progress bar, the label, and the status
    tone all stay in sync — without a single line of code wiring them up.
  </p>

  <!-- Progress bar — width is bound to a $derived percentage. -->
  <div class="bar">
    <div class="fill" style="width: {progress}%"></div>
  </div>
  <p class="meta">
    Step <strong>{step}</strong> of {TOTAL_STEPS} ·
    <span class="label">{stepLabel}</span> ·
    {Math.round(progress)}% complete ·
    <span class="status {status.tone}">{status.text}</span>
  </p>

  <div class="controls">
    <button onclick={back} disabled={step === 1}>← back</button>
    <button onclick={next} disabled={step === TOTAL_STEPS}>next →</button>
    <button class="ghost" onclick={reset}>reset</button>
  </div>

  <p class="hint">{stepsRemaining} step{stepsRemaining === 1 ? '' : 's'} remaining</p>

  <hr />

  <details open>
    <summary>What's reactive here?</summary>
    <ul>
      <li><code>step</code> — the only piece of <code>$state</code>. Everything else flows from it.</li>
      <li><code>progress</code> — <code>$derived</code> from <code>step</code>.</li>
      <li><code>stepLabel</code> — <code>$derived</code> from <code>step</code> (indexes into a const array).</li>
      <li><code>status</code> — <code>$derived.by()</code> from <code>step</code> (multi-line body, returns an object).</li>
      <li><code>stepsRemaining</code> — <code>$derived</code> from <code>step</code> (and the const <code>TOTAL_STEPS</code>).</li>
    </ul>
    <p>Five reactive expressions. One source of truth. Zero manual wiring.</p>
  </details>

  <details>
    <summary>What's the connection to <code>atopic_helper</code>?</summary>
    <p>
      Open <code>../atopic_helper/src/routes/+page.svelte</code>. You'll find:
    </p>
    <pre>const TOTAL_STEPS = 6;
let step = $state(1);
const progress = $derived(((step - 1) / (TOTAL_STEPS - 1)) * 100);</pre>
    <p>
      Same shape as this exercise. The real codebase also uses
      <code>$derived</code> for filtering allergen lists, computing display
      labels, and resolving the user's selected day across stores.
    </p>
  </details>
</main>

<style>
  main { max-width: 720px; margin: 3rem auto; padding: 0 1.5rem; font-family: system-ui, sans-serif; color: #1a1a1a; }
  .back a { font-size: 0.9rem; color: #5b6470; text-decoration: none; }
  .back a:hover { color: #c84a31; }
  h1 { font-size: 1.6rem; }
  h1 code { font-size: 0.95em; background: #f4efe4; padding: 0 0.3rem; border-radius: 3px; }
  .lede { color: #5b6470; }

  .bar { height: 14px; background: #e6e1d6; border-radius: 7px; overflow: hidden; margin: 1.5rem 0 0.6rem; }
  .fill { height: 100%; background: linear-gradient(90deg, #3a8055, #c84a31); transition: width 250ms ease-out; }

  .meta { font-size: 0.95rem; color: #5b6470; }
  .meta .label { color: #1a1a1a; font-weight: 500; }
  .status { display: inline-block; padding: 0.05rem 0.5rem; border-radius: 3px; font-size: 0.85rem; }
  .status.cool { background: #e6f0f5; color: #2d4f6c; }
  .status.warm { background: #f6e6c8; color: #8c6a14; }
  .status.hot { background: #fbe2dc; color: #c84a31; }
  .status.done { background: #d8eadb; color: #3a8055; }

  .controls { display: flex; gap: 0.6rem; margin: 1.2rem 0; }
  button { font-size: 0.95rem; padding: 0.4rem 0.9rem; border-radius: 4px; border: 1px solid #1a1a1a; background: #fbf9f4; cursor: pointer; }
  button:hover:not(:disabled) { background: #f4efe4; }
  button:disabled { opacity: 0.4; cursor: not-allowed; }
  button.ghost { border-color: #5b6470; color: #5b6470; background: transparent; }

  .hint { font-size: 0.85rem; color: #5b6470; margin: 0.5rem 0 1rem; }

  details { margin-top: 1.2rem; padding: 0.8rem 1rem; background: #fbf9f4; border: 1px solid #e6e1d6; border-radius: 4px; }
  summary { cursor: pointer; font-weight: 600; }
  pre { background: #fff; border: 1px solid #e6e1d6; padding: 0.6rem 0.8rem; border-radius: 4px; font-size: 0.85rem; overflow-x: auto; }
  ul { margin: 0.5rem 0; padding-left: 1.2rem; }
  li { padding: 0.15rem 0; }
</style>
