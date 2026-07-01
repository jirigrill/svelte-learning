<script lang="ts">
  // ════════════════════════════════════════════════════════════════
  // Lesson 0001 · $state — reactive variables
  // ════════════════════════════════════════════════════════════════
  // Two counters side-by-side. ONE uses $state, the other doesn't.
  // Click both. Watch one update, the other freeze. That is the rune.
  //
  // This file is a copy/adaptation of the pattern used in
  // atopic_helper/src/routes/+page.svelte (e.g. `let step = $state(1)`).
  // Mutate THIS file freely. The real project stays untouched.

  // Reactive: wrapped in $state — Svelte tracks reads and re-renders on writes.
  let reactiveCount = $state(0);

  // Plain: a normal JS variable. The compiler does nothing special with it.
  let plainCount = 0;

  // A reactive array — to demonstrate that mutating in place also works
  // because $state proxies the array. (atopic_helper prefers spread/filter
  // for the same effect — both styles are valid.)
  let log = $state<string[]>([]);

  function clickReactive() {
    reactiveCount++;
    log.push(`reactive → ${reactiveCount}`);
  }

  function clickPlain() {
    plainCount++;
    // We *do* mutate `log` reactively so we can prove the click handler ran,
    // even when `plainCount` itself doesn't make the UI update.
    log.push(`plain → ${plainCount} (DOM still shows old value)`);
  }

  function reset() {
    reactiveCount = 0;
    plainCount = 0;
    log = []; // assignment also triggers reactivity, same as `log.length = 0`
  }

  // ─── Bonus: the destructuring trap ──────────────────────────────
  // A reactive object. Mutating its fields *should* update the UI…
  const INITIAL_NAME = 'Jiri';
  let user = $state({ name: INITIAL_NAME, clicks: 0 });

  // …but if we destructure a field out, the local copy is frozen.
  // This is JS semantics, not Svelte's fault: destructuring assigns a
  // snapshot value to a new binding. The new binding is no longer
  // wired to the reactive Proxy on `user`.
  //
  // ⚠ DEMO CODE: this `name` binding exists ONLY so we can render it
  // alongside `user.name` and see the trap in action. In real code you
  // would not write this line — read `user.name` directly and stay
  // reactive. Delete this binding and the right-hand card has nothing
  // to display, which is exactly the point.
  const { name } = user;

  function bumpUserClicks() {
    user.clicks++;                                            // through the Proxy → triggers re-render
    user.name = INITIAL_NAME + ' ' + '!'.repeat(user.clicks); // also through the Proxy
  }
</script>

<main>
  <p class="back"><a href="/">← lesson index</a></p>
  <h1>Lesson 1 · <code>$state</code></h1>
  <p class="lede">
    Click each button. Notice that the <strong>plain</strong> counter's display
    never changes — even though the click handler runs (see the log).
  </p>

  <div class="grid">
    <section class="card reactive">
      <h2>With <code>$state</code></h2>
      <button onclick={clickReactive}>clicked {reactiveCount} times</button>
      <p class="hint">UI updates because <code>$state(0)</code> registers a dependency on every read.</p>
    </section>

    <section class="card plain">
      <h2>Without <code>$state</code></h2>
      <button onclick={clickPlain}>clicked {plainCount} times</button>
      <p class="hint">UI is frozen. <code>plainCount</code> is a normal variable; Svelte has no idea it changed.</p>
    </section>
  </div>

  <button class="reset" onclick={reset}>reset both</button>

  <h3>Event log</h3>
  <p class="hint">This list is itself <code>$state</code>, so it updates on every click — even when the plain counter's display doesn't.</p>
  <ol class="log">
    {#each log as entry, i}
      <li><span class="i">{i + 1}.</span> {entry}</li>
    {:else}
      <li class="empty">click a button…</li>
    {/each}
  </ol>

  <hr />

  <h2>Bonus · the destructuring trap</h2>
  <p class="lede">
    Click the button. <code>user.name</code> updates live; the destructured
    <code>name</code> stays frozen. Same data, two different reactivity paths.
  </p>

  <div class="grid">
    <section class="card reactive">
      <h2>Through the Proxy</h2>
      <p style="font-size: 1.4rem; margin: 0.5rem 0;">{user.name}</p>
      <p class="hint">Clicks: {user.clicks}</p>
      <p class="hint">Reads <code>user.name</code> &mdash; goes through the reactive Proxy. Updates.</p>
    </section>

    <section class="card plain">
      <h2>After destructuring</h2>
      <p style="font-size: 1.4rem; margin: 0.5rem 0;">{name}</p>
      <p class="hint">&nbsp;</p>
      <p class="hint">Reads the local <code>name</code> binding &mdash; a snapshot taken once. Frozen.</p>
    </section>
  </div>

  <button class="reset" onclick={bumpUserClicks}>bump user</button>

  <details>
    <summary>Why doesn't <code>const</code> matter here?</summary>
    <p>
      You might think <code>const &#123; name &#125; = user</code> is frozen because of <code>const</code>.
      It's not. <code>const</code> only forbids re-binding the name &mdash; the data behind a
      <code>const</code> object is fully mutable (<code>const obj = &#123;&#125;; obj.x = 1</code> works fine).
    </p>
    <p>
      The freeze comes from <strong>destructuring</strong>: it copies the value out of
      the Proxy into a new variable. To stay reactive, always read through the source:
      <code>&#123;user.name&#125;</code> not <code>&#123;name&#125;</code>.
    </p>
  </details>


  <details>
    <summary>What's the connection to <code>atopic_helper</code>?</summary>
    <p>
      In <code>atopic_helper/src/routes/+page.svelte</code> you have lines like:
    </p>
    <pre>let step = $state(1);
let motherAllergies = $state&lt;string[]&gt;([]);</pre>
    <p>
      Same rune, real domain values. <code>step</code> drives which questionnaire
      step is shown; <code>motherAllergies</code> drives which allergens appear
      selected. If you removed the <code>$state(...)</code> wrapper, the page would
      go stale exactly like the plain counter above.
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

  .grid { display: grid; grid-template-columns: 1fr 1fr; gap: 1rem; margin: 1.5rem 0; }
  .card { border: 1px solid #e6e1d6; border-radius: 6px; padding: 1rem 1.1rem; background: #fff; }
  .card h2 { margin: 0 0 0.6rem; font-size: 1rem; }
  .card.reactive { border-left: 4px solid #3a8055; }
  .card.plain { border-left: 4px solid #c84a31; }
  .card button { font-size: 1rem; padding: 0.5rem 0.9rem; border-radius: 4px; border: 1px solid #1a1a1a; background: #fbf9f4; cursor: pointer; }
  .card button:hover { background: #f4efe4; }
  .hint { font-size: 0.85rem; color: #5b6470; margin: 0.6rem 0 0; }

  .reset { margin: 0.5rem 0 1.5rem; font-size: 0.85rem; padding: 0.3rem 0.7rem; border-radius: 4px; border: 1px solid #5b6470; background: transparent; cursor: pointer; color: #5b6470; }

  .log { font-family: ui-monospace, Menlo, monospace; font-size: 0.85rem; padding-left: 0; list-style: none; background: #fbf9f4; border: 1px solid #e6e1d6; border-radius: 4px; padding: 0.6rem 0.9rem; max-height: 200px; overflow-y: auto; }
  .log li { padding: 0.15rem 0; }
  .log .i { color: #9aa3ad; display: inline-block; width: 2.5rem; }
  .log .empty { color: #9aa3ad; font-style: italic; }

  details { margin-top: 1.5rem; padding: 0.8rem 1rem; background: #fbf9f4; border: 1px solid #e6e1d6; border-radius: 4px; }
  summary { cursor: pointer; font-weight: 600; }
  pre { background: #fff; border: 1px solid #e6e1d6; padding: 0.6rem 0.8rem; border-radius: 4px; font-size: 0.85rem; overflow-x: auto; }
</style>
