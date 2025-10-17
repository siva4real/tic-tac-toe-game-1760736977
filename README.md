# Tic Tac Toe

## Summary
A minimal, modern, fully functional Tic Tac Toe game built with plain HTML, CSS, and JavaScript. It has no external dependencies and is production-ready for static hosting (including GitHub Pages). The UI is responsive, keyboard-accessible, and supports simple game state sharing and importing.

Key features:
- Two-player local play (X vs O)
- Win and draw detection with visual highlights
- New game, swap starter, and reset scores
- Keyboard support (arrow keys to move, Enter/Space to place)
- Lightweight persistence of scores and starter in localStorage
- Optional state sharing via URL hash
- Optional import of a game state via the url query parameter (?url=...)

## Setup
No build tools or dependencies are required.

- Open index.html directly in a browser; or
- Host the repository on GitHub Pages (recommended):
  1. Commit both index.html and README.md.
  2. Enable GitHub Pages in your repository settings (Source: main branch, root).
  3. Visit the published URL (e.g., https://<your-username>.github.io/<your-repo>/).

The app works immediately after deployment.

## Usage
- Click or tap a cell to place your mark (X starts by default).
- The game announces wins and draws and highlights the winning line.
- Controls:
  - New Game: starts a new round (alternates the starting player).
  - Swap Starter: switches which player starts the current/next round.
  - Reset Scores: clears X/O win counters and draws, and resets the starter to X.
- Keyboard:
  - Arrow keys to move focus within the grid.
  - Enter/Space to place a mark.

State sharing (optional):
- Click “Share” to copy a URL with the current game state encoded in the hash. Visiting that URL restores the board and whose turn it is.

Importing game state via ?url=... (optional):
- You can supply a remote JSON file containing a game state using a url query parameter:
  - Example: https://your-pages-site.example/index.html?url=https://example.com/game.json
  - The JSON schema must be:
    {
      "board": ["X","O","","","X","","","O",""],  // 9 entries, "", "X", or "O"
      "current": "X"                               // whose turn next
    }
  - Notes and behavior:
    - Only http(s) URLs are attempted.
    - The file size is limited to 50 KB.
    - CORS must allow the request (typical for raw hosting/CDN).
    - If the imported position is already a win or a draw, the app will show that state and prevent further moves for that round.
    - Importing does not change the win counters.

The app ignores invalid or unreachable URLs and will start a fresh game instead.

## Code Explanation
- index.html
  - Contains all HTML, CSS, and JavaScript in a single, production-ready file. No external resources.
  - Accessibility:
    - The board uses buttons with clear aria-labels per cell and a live status region.
    - Keyboard navigation within the grid with arrow keys; Enter/Space to play.
  - State:
    - board: array of 9 strings "", "X", or "O".
    - current: "X" or "O", indicating whose turn it is.
    - gameOver: boolean; true after a win or draw.
    - winLine: array of indexes of the winning cells (for highlight).
    - starter: which player starts the next round (alternates by default).
    - stats: { X, O, D } counts wins for X/O and draws.
    - stats and starter are persisted to localStorage.
  - Logic:
    - Win detection checks 8 combinations (rows, columns, diagonals).
    - Click handler writes to board, re-renders, and updates status/score.
    - New Game clears the board and alternates starter.
    - Swap Starter toggles starter immediately.
    - Reset Scores clears stats and resets starter to X.
  - Rendering:
    - Each cell’s text content and classes are updated on each move.
    - Disabled state prevents overwriting moves and interaction after gameOver.
    - Winning cells receive a glow highlight.
  - URL handling:
    - Share link encodes the current state into the URL hash (state=...).
    - On load, the app first tries to restore from hash; if absent, it checks for a url query parameter (?url=...).
    - If ?url=... is present, the app fetches JSON, validates shape, and applies it. Errors are caught and do not break the UI.
    - Safety measures include protocol checks (http/https only), a 4-second timeout, and a 50 KB payload limit.

## License
MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.