<script lang="ts">
  // Mirror of how atopic_helper structures pages: TS in <script>, runes for state.
  // This is the lesson index — pure navigation, no reactive state needed yet.

  type Lesson = { num: string; slug: string; title: string; status: 'ready' | 'todo' };

  const lessons: Lesson[] = [
    { num: '0001', slug: 'state', title: '$state — reactive variables', status: 'ready' },
    { num: '0002', slug: 'derived', title: '$derived — computed values', status: 'ready' },
    { num: '0003', slug: 'effect', title: '$effect — side effects', status: 'todo' }
  ];
</script>

<main>
  <h1>Svelte learning · playground</h1>
  <p class="lede">
    Each lesson has its own route. Open <code>lessons/0001-state-rune.html</code> in
    <code>../lessons/</code> for the explanation, then come here to run the exercise.
  </p>
  <ul>
    {#each lessons as lesson}
      <li>
        <span class="num">{lesson.num}</span>
        {#if lesson.status === 'ready'}
          <a href="/lesson/{lesson.slug}">{lesson.title}</a>
        {:else}
          <span class="todo">{lesson.title} <em>(coming soon)</em></span>
        {/if}
      </li>
    {/each}
  </ul>
</main>

<style>
  main { max-width: 640px; margin: 4rem auto; padding: 0 1.5rem; font-family: system-ui, sans-serif; color: #1a1a1a; }
  h1 { font-size: 1.6rem; border-bottom: 2px solid #1a1a1a; padding-bottom: 0.4rem; }
  .lede { color: #5b6470; }
  ul { list-style: none; padding: 0; }
  li { padding: 0.6rem 0; border-bottom: 1px solid #e6e1d6; display: flex; gap: 0.8rem; align-items: baseline; }
  .num { font-family: ui-monospace, Menlo, monospace; font-size: 0.85rem; color: #c84a31; }
  .todo { color: #9aa3ad; }
  a { color: #2d4f6c; text-decoration: none; border-bottom: 1px solid #2d4f6c; }
  a:hover { color: #c84a31; border-color: #c84a31; }
</style>
