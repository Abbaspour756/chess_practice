# Chess

A single-file, browser-based chess game with a built-in AI opponent. Pick a difficulty, pick your color, and play — no installation, build step, or server required.

## Features

- **Setup screen** — choose the opponent's difficulty (Easy / Medium / Hard) and your piece color (White / Black / Random) before the game starts.
- **Full chess rules** — legal move generation, check, checkmate, stalemate, draw by repetition, draw by insufficient material, and the fifty-move rule, all handled via `chess.js`.
- **Click-to-move interface** — click a piece to see its legal destinations highlighted (a dot for a quiet move, a ring for a capture), then click a destination to move.
- **Pawn promotion** — a modal lets you choose Queen, Rook, Bishop, or Knight when a pawn reaches the last rank.
- **AI opponent** — a minimax search with alpha-beta pruning and piece-square-table evaluation, written from scratch (not part of `chess.js`).
- **Game info panel** — turn indicator, check warnings, captured-piece tracker, full move history, undo, board flip, and a new-game reset.
- **Responsive layout** — works on both desktop and mobile screen sizes.

## Getting started

1. Download `chess.html`.
2. Open it in any modern browser (Chrome, Firefox, Safari, Edge).
3. Pick a difficulty and a color, then click **Start game**.

That's it — everything runs client-side in the browser.

## How the AI works

The engine treats the board position with a classic **material + positional** evaluation:

- **Material** — standard piece values (pawn 100, knight 320, bishop 330, rook 500, queen 900).
- **Position** — piece-square tables nudge each piece toward squares that are generally stronger for it (e.g. knights toward the center, pawns toward advancing safely).

On top of that evaluation, the bot searches ahead using **minimax with alpha-beta pruning**, with the search depth controlled by difficulty:

| Difficulty | Behavior |
|---|---|
| Easy | Mostly random legal moves, with a mild bias toward capturing when a capture is available. No lookahead. |
| Medium | Minimax search, 2 plies deep. |
| Hard | Minimax search, 3 plies deep, with move ordering (captures searched first) to prune more efficiently. |

Moves are ordered so that captures are evaluated first, which lets alpha-beta pruning cut off more of the search tree and makes the higher difficulty levels noticeably stronger without needing a deeper raw search.

## Tech stack

- **HTML** — page structure and layout.
- **CSS** (vanilla, no framework) — all styling, including the board, panels, and animations.
- **JavaScript** (vanilla, no framework) — game state, board rendering, click handling, and the AI engine.
- **[chess.js](https://github.com/jhlywa/chess.js)** (loaded from cdnjs) — the only external dependency; handles move legality, check/checkmate detection, and game-state rules. Everything else, including the AI, is custom code.

## File structure

This is a single self-contained HTML file:

```
chess.html
├── <style>   → all CSS (design tokens, layout, board, sidebar, animations)
├── <body>    → setup screen + game screen + promotion modal markup
└── <script>  → game state, rendering, move handling, and the minimax AI
```

## Customizing

A few easy things to tweak directly in the file:

- **Search depth** — change the `DEPTH_BY_DIFF` object to make any difficulty level search deeper (stronger, but slower) or shallower (weaker, but faster).
- **Piece values / evaluation** — adjust `PIECE_VALUE` or the piece-square tables (`PST_PAWN`, `PST_KNIGHT`, etc.) to change how the AI values material and position.
- **Colors and fonts** — all visual styling is driven by CSS custom properties at the top of the `<style>` block (`--brass`, `--board-light`, `--board-dark`, etc.), so the theme can be restyled without touching layout code.

## Known limitations

- No online multiplayer — this is a local, single-browser-tab game against the built-in AI.
- No opening book or endgame tablebase — the AI relies purely on live search plus the evaluation function, so it can occasionally make weaker choices deep in the endgame.
- Progress is not saved between page reloads; refreshing the page resets the game.
