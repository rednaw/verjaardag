<script>
  import { base } from '$app/paths';
  import { onMount } from 'svelte';

  let game1Complete = false;
  let shakeGame2 = false;

  onMount(() => {
    game1Complete = localStorage.getItem('game1Complete') === 'true';
  });

  function handleGame2Click(event) {
    if (!game1Complete) {
      event.preventDefault();
      shakeGame2 = true;
      setTimeout(() => {
        shakeGame2 = false;
      }, 400);
    }
  }
</script>

<main>
  <div class="content">
    <div class="games-column">
      <a href="{base}/game1" class="game-button">1</a>
      <div class="arrow">↓</div>
      <a
        href="{base}/game2"
        class="game-button {shakeGame2 ? 'shake' : ''} {game1Complete ? '' : 'disabled'}"
        on:click={handleGame2Click}
        tabindex="0"
        aria-disabled={!game1Complete}
      >2</a>
    </div>
  </div>
</main>

<style>
  main {
    text-align: center;
    min-height: 100vh;
    display: flex;
    background: var(--color-background);
  }

  .content {
    width: 100%;
    padding: 2.5rem 1rem 1.5rem 1rem;
    display: flex;
    flex-direction: column;
    justify-content: center;
    flex: 1;
    margin-top: 0;
  }

  .games-column {
    display: flex;
    flex-direction: column;
    align-items: center;
    justify-content: center;
    gap: 1.2rem;
    max-width: 400px;
    width: 100%;
    margin: 0 auto;
  }

  .game-button {
    color: var(--color-text);
    text-decoration: none;
    font-size: 1.5rem;
    padding: 1.2rem;
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
    min-width: 70px;
    min-height: 70px;
    max-width: 120px;
    max-height: 120px;
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

  .arrow {
    font-size: 2.5rem;
    color: var(--color-text);
    margin: 0.5rem 0;
  }

  .game-button.disabled {
    pointer-events: auto;
    opacity: 0.5;
    cursor: not-allowed;
  }
  .game-button.shake {
    animation: shake 0.4s;
  }
  @keyframes shake {
    0% { transform: translateX(0); }
    20% { transform: translateX(-8px); }
    40% { transform: translateX(8px); }
    60% { transform: translateX(-8px); }
    80% { transform: translateX(8px); }
    100% { transform: translateX(0); }
  }

  @media (max-width: 500px) {
    .content {
      padding: 1.2rem 0.5rem 1rem 0.5rem;
    }
    .games-column {
      gap: 0.7rem;
    }
    .game-button {
      padding: 0.7rem;
      font-size: 1.1rem;
      min-width: 48px;
      min-height: 48px;
      max-width: 80px;
      max-height: 80px;
    }
    .arrow {
      font-size: 1.7rem;
      margin: 0.3rem 0;
    }
  }
</style> 