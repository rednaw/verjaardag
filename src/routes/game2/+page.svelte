<script>
  import { base } from '$app/paths';
  import { words } from '../words.js';
</script>

<div class="page-wrapper">
  <a href="{base}/" class="back-arrow">←</a>
  <main>
    <div class="word-list">
      {#each words as word}
        <div class="word">{word}</div>
      {/each}
    </div>
  </main>
</div>

<style>
  .page-wrapper {
    width: 100vw;
    min-height: 100vh;
    background: var(--color-background);
    position: relative;
    overflow-x: hidden;
  }
  .back-arrow {
    position: absolute;
    top: 1.2rem;
    left: 1.2rem;
    font-size: 2.5rem;
    color: #e53935;
    text-decoration: none;
    font-weight: bold;
    z-index: 100;
    transition: color 0.2s;
  }
  .back-arrow:hover {
    color: #b71c1c;
  }
  main {
    display: flex;
    flex-direction: column;
    align-items: center;
    min-height: 100vh;
    background: var(--color-background);
    padding-top: 0.5rem;
  }
  .word-list {
    display: flex;
    flex-direction: column;
    align-items: center;
    gap: 0.7rem;
    margin-top: 4.2rem;
    width: 100%;
    max-width: 400px;
  }
  .word {
    font-size: 1.3rem;
    letter-spacing: 0.1em;
    color: var(--color-text);
    background: var(--color-white);
    padding: 0.4rem 1.2rem;
    border-radius: 0.5rem;
    box-shadow: 0 1px 4px rgba(0,0,0,0.04);
    margin-bottom: 0.2rem;
    white-space: nowrap;
    text-align: center;
    width: 100%;
  }
</style> 