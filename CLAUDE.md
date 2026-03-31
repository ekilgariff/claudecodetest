# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the games

Open any `.html` file directly in a browser — no build step, server, or dependencies needed.

## Git workflow

**After every meaningful change, commit and push immediately.** Never leave work uncommitted. This ensures we always have a saved state on GitHub and can revert to any point.

```bash
git add <changed-files>
git commit -m "descriptive message"
git push
```

Commit message rules:
- Use a short imperative subject line (e.g. `Add power-up system to shooter`)
- Be specific — describe what changed and why, not just "update file"
- Always include the Co-Authored-By trailer for Claude commits

Remote: https://github.com/ekilgariff/claudecodetest

## Architecture

Each game is a single self-contained `.html` file (HTML + CSS + JS, no external dependencies).

### shooter.html

Canvas-based top-down shooter (800×600). Key sections in order:

- **Setup** — canvas, 2D context, constants `W`/`H`
- **Input** — `keys` object for WASD/arrows, `mouse` for aim and fire
- **Game state** — `state` FSM: `menu → playing → levelComplete → gameOver`
- **Level config** (`getLvlCfg`)  — scales enemy count, speed, HP, and available types per level
- **Enemy factory** (`spawnEnemy`) — three types: `walker` (basic), `charger` (fast, 1 HP), `shooter` (slow, ranged)
- **Update loop** — player movement → enemy spawning → enemy AI → bullet collision → particle/popup tick
- **Draw loop** — background → player → enemies → bullets → particles → HUD → overlays
- **Enemy sprites** — drawn in `drawEnemy()` using a 🌮 emoji scaled to `e.size * 2.2`, rotated to face the player; HP bar rendered above when damaged
- **Persistence** — hi-score stored in `localStorage` under key `gnr_hi`

### tictactoe.html

DOM-based 2-player Tic Tac Toe. State is a flat 9-element array (`board`). Win detection checks the 8 hardcoded `WINS` combos. Scores persist in memory only (reset on page reload).
