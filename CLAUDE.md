# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Vanilla JavaScript Tetris. No dependencies, no build system. Entire game logic in `game.js`. Modern ES6+ with requestAnimationFrame game loop. Spanish UI text but code in English.

Single HTML page with two canvases: main board (10×20 grid) and next‑piece preview.

## Architecture

`game.js` is monolithic: contains board model, piece definitions, collision detection, rotation with wall kicks, scoring, rendering, input handling, and game state. Tight coupling to DOM — directly references canvas contexts and UI elements by ID. No class abstractions.

Key data structures:
- `board`: ROWS×COLS matrix of integers (0 = empty, 1–7 = piece color index)
- `current`, `next`: objects with `type`, `shape` (2D array), `x`, `y`
- `PIECES`: array of 2D shape definitions; first element null, types 1–7 correspond to colors in `COLORS`

Game state vars: `score`, `lines`, `level`, `paused`, `gameOver`, `dropInterval`.

Rendering: `draw()` updates main canvas with board, ghost piece, current piece. `drawGrid()` draws cell borders. `drawBlock()` renders individual colored blocks with highlight effect.

## How to run

Open `index.html` directly in browser, or start a static file server:

```bash
python3 -m http.server 8000
# or
npx serve .
```

Access `http://localhost:8000`.

No build, no lint, no tests. Game works standalone.

## Key constants (tunable)

- `COLS = 10`, `ROWS = 20`, `BLOCK = 30` (pixel size per cell)
- `COLORS`: palette for each piece type
- `LINE_SCORES = [0, 100, 300, 500, 800]` (classic Tetris scoring)
- `dropInterval`: base fall speed (1000ms) decreases with level

Changing grid dimensions requires adjusting canvas `width`/`height` in `index.html`.

## Design notes

- `tryRotate()` implements basic wall kicks: offsets [0, -1, 1, -2, 2]
- `ghostY()` computes landing position for translucent ghost piece
- Scoring: soft drop = 1 point per row, hard drop = 2 points per row skipped
- Level increases every 10 lines, speed formula: `max(100, 1000 - (level - 1) * 90)`
- UI in Spanish (`Puntuación`, `Reiniciar`), controls legend in Spanish
- Dark retro aesthetic with backdrop blur overlays