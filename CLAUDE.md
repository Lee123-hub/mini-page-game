# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Running the Project

No build step — open `index.html` directly in a browser, or serve the directory:

```bash
python3 -m http.server
```

Then visit `http://localhost:8000`.

## Architecture

**`index.html`** is the portal. It defines a `GAMES` JavaScript array where each entry has `title`, `emoji`, `bg` (gradient), `url`, and `rules` (tooltip text). The page renders these as clickable cards dynamically. To add a new game, append an entry to this array.

**`games/*.html`** — each game is a fully self-contained HTML file with all CSS and JS inline. There are no shared files, no imports, and no build pipeline. Navigation back to the portal uses a hardcoded `<a href="../index.html">` link.

## Shared Patterns (duplicated across all game files)

All game pages follow the same structure by convention:

- **Dark theme**: background `#0f0f1a`, topbar `#1a1a2e`, accent gradient `linear-gradient(135deg, #667eea, #764ba2)`
- **Canvas setup**: `setupCanvas()` applies `devicePixelRatio` scaling for sharp rendering
- **Modals**: toggled via `classList.add/remove('hidden')` on `.modal` elements
- **AI trigger**: `triggerAI()` wraps the AI call in `setTimeout(..., 50–60ms)` to yield to the browser for a repaint before computing
- **Mode/difficulty**: `selectMode('2p'|'1p')` and `selectDiff('low'|'mid'|'high')` toggle `.active` class on buttons

## AI Difficulty Levels

All four games implement the same three-tier AI pattern:
- **Low**: random legal move
- **Mid**: 1-ply greedy (material gain / immediate win-block)
- **High**: negamax or minimax with alpha-beta pruning, depth 3

## Known Gap

`games/checkers.html` is fully implemented but **not listed in the `GAMES` array** in `index.html`, making it unreachable from the portal. Add an entry to make it accessible.

## Git Push

This project uses a dedicated SSH key stored at `.ssh/github_minipage` (relative to the repo root). Always push with:

```bash
GIT_SSH_COMMAND="ssh -i /Users/lixin156/Desktop/mini-page-game/.ssh/github_minipage -o StrictHostKeyChecking=no" git push origin main
```

If the push is rejected due to remote changes, rebase first:

```bash
GIT_SSH_COMMAND="ssh -i /Users/lixin156/Desktop/mini-page-game/.ssh/github_minipage -o StrictHostKeyChecking=no" git pull --rebase origin main
```

Remote: `git@github.com:Lee123-hub/mini-page-game.git`
