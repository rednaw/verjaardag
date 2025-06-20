<script>
  import { base } from '$app/paths';
</script>

<a href="{base}/" class="back-arrow">←</a>
<main>
  <h1>Game 2 (Coming Soon)</h1>
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
</style> 