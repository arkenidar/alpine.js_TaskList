# Tic-Tac-Toe: Three-Version Comparison

A tic-tac-toe game implemented in three different JavaScript approaches, following the same comparison pattern as the [TaskList app](../index.html).

Ported from [arkenidar/js-tic-tac-toe-game](https://github.com/arkenidar/js-tic-tac-toe-game) (vanilla JS with manual DOM updates) into three variants with different reactivity patterns.

## The Three Versions

1. **[Alpine.js](index.html)** - Automatic reactivity with declarative templates
2. **[jQuery Slim](jquery.html)** - Manual reactivity with `$container.data()` storage
3. **[Vanilla JS](vanilla-js.html)** - Manual reactivity with native DOM APIs

## Project Structure

```
tic-tac-toe/
├── index.html          # Alpine.js version
├── jquery.html         # jQuery Slim version
├── vanilla-js.html     # Vanilla JS version
├── css/
│   ├── alpine.css      # Shared styles (all three identical)
│   ├── jquery.css
│   └── vanilla-js.css
├── show-source.js      # Source code viewer
└── README.md
```

## Features

All three versions implement identical functionality:

| Feature | Alpine.js | jQuery | Vanilla JS |
|---------|-----------|--------|------------|
| **3x3 clickable grid** | ✅ | ✅ | ✅ |
| **X/O alternation** | ✅ | ✅ | ✅ |
| **Winner detection** | ✅ | ✅ | ✅ |
| **Draw detection** | ✅ | ✅ | ✅ |
| **Move history** | ✅ | ✅ | ✅ |
| **Time travel** | ✅ | ✅ | ✅ |

### Gameplay

- Click a cell to place X or O (alternating turns)
- Winner detection checks 8 lines: 3 rows, 3 columns, 2 diagonals
- Draw detected when the board is full with no winner
- Move history buttons appear below the grid
- Click any history button to jump back to that state
- Making a move from a past state creates a new branch (future history is discarded)

## Reactivity Patterns

### Alpine.js: Automatic

```javascript
// Data model with computed getters
{
  grid: [['','',''], ['','',''], ['','','']],
  currentPlayer: 'X',
  history: [],
  get winner() { /* re-evaluated automatically */ },
  get statusMessage() { /* derived from state */ }
}

// Template-driven rendering
<template x-for="(row, ri) in grid">
  <td @click="makeMove(ri, ci)" x-text="cell"></td>
</template>
```

Change `grid` or `currentPlayer` and the view updates automatically.

### jQuery / Vanilla JS: Manual

```javascript
// Data stored on DOM container
$container.data('grid', grid);        // jQuery
container.game_data = { grid, ... };  // Vanilla JS

// Every state change requires explicit render
game_click($container, row, col);
game_render($container);  // MUST call to update view
```

Same manual reactivity pattern as the TaskList versions, inspired by [arkenidar/list-app](https://github.com/arkenidar/list-app).

## Original Source

Ported from [arkenidar/js-tic-tac-toe-game](https://github.com/arkenidar/js-tic-tac-toe-game), which is a vanilla JS implementation inspired by the React tutorial tic-tac-toe example.

## Running

No build step required. Open any HTML file in your browser:

```bash
open index.html       # Alpine.js
open jquery.html      # jQuery
open vanilla-js.html  # Vanilla JS
```

Each file includes `show-source.js` to display source code in-browser below the game.
