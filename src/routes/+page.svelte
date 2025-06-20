<script>
  import { base } from '$app/paths';
</script>

<main>
  <div class="content">
    <div class="games-column">
      <a href="{base}/game1" class="game-button">1</a>
      <div class="arrow">↓</div>
      <a href="{base}/game2" class="game-button">2</a>
    </div>
  </div>
</main>

<style>
  main {
    text-align: center;
    min-height: 105vh;
    display: flex;
    background: var(--color-background);
  }

  .content {
    width: 100%;
    padding: 8rem 2rem 2rem 2rem;
    display: flex;
    flex-direction: column;
    margin-top: auto;
  }

  .games-column {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 2rem;
    max-width: 400px;
    width: 100%;
    margin: 0 auto;
  }

  .game-button {
    color: var(--color-text);
    text-decoration: none;
    font-size: 2rem;
    padding: 2rem;
    background: var(--color-white);
    border: 2px solid var(--color-border);
    border-radius: 0.5rem;
    transition: all 0.2s ease;
    font-weight: 500;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    aspect-ratio: 1;
    display: flex;
    align-items: center;
    justify-content: center;
    min-width: 120px;
    min-height: 120px;
  }

  .game-button:hover {
    background: var(--color-hover-bg);
    border-color: var(--color-hover-border);
    transform: translateY(-2px);
    box-shadow: 0 4px 12px rgba(0, 0, 0, 0.15);
  }

  .game-button:active {
    transform: translateY(0);
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
  }

  @media (max-width: 500px) {
    .content {
      padding: 6rem 1rem 2rem 1rem;
    }
    .games-column {
      gap: 1rem;
    }
    .game-button {
      padding: 1.5rem;
      font-size: 1.75rem;
      min-width: 80px;
      min-height: 80px;
    }
  }

  .arrow {
    font-size: 4rem;
    color: var(--color-text);
    margin: 1rem 0;
  }
</style> 