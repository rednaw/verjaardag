<script>
  import { words } from './words.js';
  import { onMount } from 'svelte';

  const gridSize = 7;

  function maskWord(word) {
    if (word.length < 2) return word;
    const idx = Math.floor(Math.random() * word.length);
    return word.substring(0, idx) + '_' + word.substring(idx + 1);
  }
  const maskedWords = words.map(maskWord);

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

  function createEmptyGrid(size) {
    return Array.from({ length: size }, () => Array(size).fill(''));
  }

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

  function placeWord(grid, word, x, y, dx, dy) {
    for (let i = 0; i < word.length; i++) {
      const nx = x + dx * i;
      const ny = y + dy * i;
      grid[ny][nx] = word[i];
    }
  }

  function removeWord(grid, word, x, y, dx, dy) {
    for (let i = 0; i < word.length; i++) {
      const nx = x + dx * i;
      const ny = y + dy * i;
      grid[ny][nx] = '';
    }
  }

  function shuffle(array) {
    for (let i = array.length - 1; i > 0; i--) {
      const j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
  }

  function backtrack(grid, wordList, wordIdx = 0) {
    if (wordIdx === wordList.length) return true;
    const word = wordList[wordIdx];
    const positions = [];
    for (let y = 0; y < gridSize; y++) {
      for (let x = 0; x < gridSize; x++) {
        for (const [dx, dy] of directions) {
          positions.push([x, y, dx, dy]);
        }
      }
    }
    shuffle(positions);
    for (const [x, y, dx, dy] of positions) {
      if (canPlace(grid, word, x, y, dx, dy)) {
        placeWord(grid, word, x, y, dx, dy);
        if (backtrack(grid, wordList, wordIdx + 1)) return true;
        removeWord(grid, word, x, y, dx, dy);
      }
    }
    return false;
  }

  function generateGrid() {
    let grid = createEmptyGrid(gridSize);
    // Try to place all words with backtracking
    backtrack(grid, words);
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

  let grid = [];
  onMount(() => {
    grid = generateGrid();
  });

  let selected = [];
  function toggleCell(y, x) {
    const idx = selected.findIndex(([sy, sx]) => sy === y && sx === x);
    if (idx === -1) {
      selected = [...selected, [y, x]];
    } else {
      selected = selected.filter(([sy, sx]) => !(sy === y && sx === x));
    }
  }
  function isSelected(y, x) {
    return selected.some(([sy, sx]) => sy === y && sx === x);
  }

  let foundWords = [];
  $: foundWordsSet = new Set(foundWords.map(w => w.toUpperCase()));
  let foundCells = [];
  let shake = false;
  let transitionOut = false;

  function checkSelection() {
    if (selected.length < 2) {
      selected = [];
      return;
    }
    const letters = selected.map(([y, x]) => grid[y][x]).join('');
    const reversed = letters.split('').reverse().join('');
    const matchIdx = words.findIndex(
      w => !foundWords.includes(w) && (w === letters || w === reversed)
    );
    if (matchIdx !== -1) {
      foundWords = [...foundWords, words[matchIdx]];
      foundCells = [...foundCells, ...selected];
      if (foundWords.length === words.length) {
        localStorage.setItem('game1Complete', 'true');
        setTimeout(() => {
          transitionOut = true;
          setTimeout(() => {
            window.location.href = '/game2';
          }, 2000);
        }, 500);
      }
    } else {
      shake = true;
      setTimeout(() => {
        shake = false;
        selected = [];
      }, 400);
      return;
    }
    selected = [];
  }
  function isCellFound(y, x) {
    return foundCells.some(([py, px]) => py === y && px === x);
  }
</script>

<div class="page-wrapper {transitionOut ? 'dramatic-out' : ''}">
  <main>
    {#if grid.length}
      {#key foundCells.length}
        <div class="grid {shake ? 'shake' : ''}">
          {#each grid as row, y}
            <div class="row">
              {#each row as letter, x}
                <button
                  type="button"
                  class="cell {isSelected(y, x) ? 'selected' : ''} {isCellFound(y, x) ? 'found' : ''}"
                  aria-label={`Letter ${letter} at row ${y + 1}, column ${x + 1}`}
                  on:click={() => toggleCell(y, x)}
                >
                  {letter}
                </button>
              {/each}
            </div>
          {/each}
        </div>
      {/key}
      <div class="word-list">
        {#each words as word, i}
          <span class="word {foundWordsSet.has(word.toUpperCase()) ? 'found' : ''}">{foundWordsSet.has(word.toUpperCase()) ? word : maskedWords[i]}</span>
        {/each}
      </div>
      <button class="check-btn" on:click={checkSelection} disabled={selected.length === 0}>Controleer</button>
      {#if foundWords.length === words.length}
        <!-- No congratulatory message or button, just the animation and auto-transition -->
      {/if}
    {/if}
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
    max-width: 100vw;
    max-height: 80vw;
    box-sizing: border-box;
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
    cursor: pointer;
    transition: background 0.2s, border-color 0.2s;
    outline: none;
  }
  .cell.selected {
    background: #ffe082;
    border-color: #ffb300;
  }
  .cell.found {
    background: #a5d6a7;
    border-color: #388e3c;
    color: #1b5e20;
  }
  .cell:focus {
    box-shadow: 0 0 0 2px #1976d2;
    border-color: #1976d2;
  }
  .check-btn {
    margin: 2.2rem 0 0.7rem 0;
    padding: 0.7rem 2.2rem;
    font-size: 1.2rem;
    border-radius: 0.5rem;
    border: none;
    background: #1976d2;
    color: white;
    font-weight: 600;
    cursor: pointer;
    box-shadow: 0 2px 8px rgba(0,0,0,0.08);
    transition: background 0.2s;
    align-self: center;
    display: block;
  }
  .check-btn:disabled {
    background: #b0bec5;
    cursor: not-allowed;
  }
  .grid.shake {
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
    transition: background 0.2s, color 0.2s;
  }
  .word.found {
    background: #a5d6a7;
    color: #1b5e20;
    text-decoration: line-through;
    font-weight: bold;
    border: 2px solid #388e3c;
  }
  .congrats {
    margin-top: 2rem;
    font-size: 1.3rem;
    color: var(--color-exact);
    font-weight: bold;
    text-align: center;
  }
  .dramatic-out {
    animation: dramatic-fade-scale 2s forwards;
  }
  @keyframes dramatic-fade-scale {
    0% {
      opacity: 1;
      transform: scale(1) rotate(0deg);
    }
    40% {
      opacity: 1;
      transform: scale(1.1) rotate(-2deg);
    }
    70% {
      opacity: 0.7;
      transform: scale(1.2) rotate(2deg);
    }
    100% {
      opacity: 0;
      transform: scale(2) rotate(-8deg);
    }
  }
  @media (max-width: 500px) {
    .grid {
      max-width: 98vw;
      max-height: 98vw;
      padding: 0.5rem;
      gap: 0.2rem;
    }
    .cell {
      width: 11vw;
      height: 11vw;
      font-size: 1.2rem;
      min-width: 0;
      min-height: 0;
      padding: 0;
    }
  }
</style> 