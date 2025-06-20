<script>
  // Static 6x6 grid of random letters
  const grid = [
    ['Q', 'W', 'E', 'R', 'T', 'Y'],
    ['A', 'S', 'D', 'F', 'G', 'H'],
    ['Z', 'X', 'C', 'V', 'B', 'N'],
    ['U', 'I', 'O', 'P', 'L', 'K'],
    ['M', 'J', 'H', 'G', 'F', 'D'],
    ['S', 'A', 'Q', 'W', 'E', 'R']
  ];
  import { base } from '$app/paths';
</script>

<a href="{base}/" class="back-arrow">←</a>
<main>
  <h1>Word Search</h1>
  <div class="grid">
    {#each grid as row}
      <div class="row">
        {#each row as letter}
          <div class="cell">{letter}</div>
        {/each}
      </div>
    {/each}
  </div>
</main>

<style>
  .back-arrow {
    position: fixed;
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
    justify-content: center;
    min-height: 100vh;
    background: var(--color-background);
  }
  h1 {
    margin-bottom: 2rem;
  }
  .grid {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    background: var(--color-white);
    padding: 2rem;
    border-radius: 1rem;
    box-shadow: 0 4px 32px rgba(0,0,0,0.1);
  }
  .row {
    display: flex;
    gap: 0.5rem;
  }
  .cell {
    width: 2.5rem;
    height: 2.5rem;
    display: flex;
    align-items: center;
    justify-content: center;
    font-size: 2rem;
    font-weight: bold;
    background: var(--color-background);
    border: 2px solid var(--color-border);
    border-radius: 0.5rem;
    user-select: none;
  }
</style> 