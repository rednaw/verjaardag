<script>
  import { base } from '$app/paths';

  // Updated list of words to hide in the grid
  const words = [
    'OPA',
    'LAGOS',
    'PANAGA',
    'ROLDE',
    'PROBUS',
    'BILJART'
  ];
  const gridSize = 7;

  // Directions: [dx, dy]
  const directions = [
    [1, 0],   // right
    [0, 1],   // down
    [1, 1],   // diagonal down-right
    [-1, 1],  // diagonal down-left
    [-1, 0],  // left
    [0, -1],  // up
    [1, -1],  // diagonal up-right
    [-1, -1], // diagonal up-left
  ];

  // Helper to create an empty grid
  function createEmptyGrid(size) {
    return Array.from({ length: size }, () => Array(size).fill(''));
  }

  // Helper to check if a word fits at a position in a direction
  function canPlace(grid, word, x, y, dx, dy) {
    for (let i = 0; i < word.length; i++) {
      const nx = x + dx * i;
      const ny = y + dy * i;
      if (
        nx < 0 || nx >= gridSize ||
        ny < 0 || ny >= gridSize ||
        (grid[ny][nx] && grid[ny][nx] !== word[i])
      ) {
        return false;
      }
    }
    return true;
  }

  // Helper to place a word in the grid
  function placeWord(grid, word) {
    const tries = 100;
    for (let t = 0; t < tries; t++) {
      const dir = directions[Math.floor(Math.random() * directions.length)];
      const dx = dir[0];
      const dy = dir[1];
      const maxX = dx === 0 ? gridSize - 1 : dx > 0 ? gridSize - word.length : word.length - 1;
      const maxY = dy === 0 ? gridSize - 1 : dy > 0 ? gridSize - word.length : word.length - 1;
      const x = Math.floor(Math.random() * (maxX + 1));
      const y = Math.floor(Math.random() * (maxY + 1));
      if (canPlace(grid, word, x, y, dx, dy)) {
        for (let i = 0; i < word.length; i++) {
          grid[y + dy * i][x + dx * i] = word[i];
        }
        return true;
      }
    }
    return false;
  }

  // Generate the grid with all words
  function generateGrid() {
    let grid = createEmptyGrid(gridSize);
    for (const word of words) {
      placeWord(grid, word);
    }
    // Fill empty cells with random letters
    for (let y = 0; y < gridSize; y++) {
      for (let x = 0; x < gridSize; x++) {
        if (!grid[y][x]) {
          grid[y][x] = String.fromCharCode(65 + Math.floor(Math.random() * 26));
        }
      }
    }
    return grid;
  }

  const grid = generateGrid();
</script>

<div class="page-wrapper">
  <a href="{base}/" class="back-arrow">←</a>
  <main>
    <div class="grid">
      {#each grid as row}
        <div class="row">
          {#each row as letter}
            <div class="cell">{letter}</div>
          {/each}
        </div>
      {/each}
    </div>
    <div class="word-list">
      {#each words as word}
        <span class="word">{word}</span>
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
  .grid {
    display: flex;
    flex-direction: column;
    gap: 0.5rem;
    background: var(--color-white);
    padding: 1.2rem 1.2rem 1.2rem 1.2rem;
    border-radius: 1rem;
    box-shadow: 0 4px 32px rgba(0,0,0,0.1);
    margin-bottom: 1.5rem;
    margin-top: 4.2rem;
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
  .word-list {
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    align-items: center;
    gap: 0.5rem;
    margin-top: 0.5rem;
    width: 100%;
    max-width: 400px;
  }
  .word {
    font-size: 1.1rem;
    letter-spacing: 0.1em;
    color: var(--color-text);
    background: var(--color-white);
    padding: 0.3rem 0.8rem;
    border-radius: 0.5rem;
    box-shadow: 0 1px 4px rgba(0,0,0,0.04);
    margin-bottom: 0.2rem;
    white-space: nowrap;
  }
</style> 