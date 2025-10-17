# Red Preview

## Summary
Red Preview is a minimal, production-ready, red-themed web application for quickly embedding and previewing external URLs. It reads the `?url=` query parameter and attempts to display the provided page in an iframe. If the target site blocks embedding, the app provides an “Open in new tab” fallback and shareable link support.

Key characteristics:
- Single-file app (index.html) with no external dependencies
- Modern, clean, red-themed UI
- Handles `?url=...` query parameter
- Works out of the box on GitHub Pages

Example:
- https://your-username.github.io/your-repo/?url=https%3A%2F%2Fexample.com

## Setup
1. Create a new repository (or use an existing one).
2. Add the provided `index.html` to the repository root.
3. Commit and push.
4. Enable GitHub Pages for the repository:
   - Repository Settings → Pages → Build and deployment → Source: “Deploy from a branch”
   - Branch: “main” (or your default branch), Folder: “/ (root)”
   - Save. After it builds, your app will be live at the shown Pages URL.

No build steps, no package installs, and no external assets are required.

## Usage
- Interactive:
  1. Navigate to the app.
  2. Paste a URL into the input and press Enter or click “Preview”.
  3. If the site allows embedding, it will render below. If it doesn’t, use “Open in new tab”.

- Via query parameter:
  - Append `?url=<encoded URL>` to the app URL. For example:
    - `?url=https%3A%2F%2Fexample.com`
  - The app will auto-load and attempt to embed the target site.
  - The input field is pre-filled, and the “Open in new tab” link is set.

- Share a link:
  1. Enter a URL and click “Copy Link”. A shareable URL with the `?url=` parameter is copied to your clipboard.

- Clear:
  - Click “Clear” to reset the viewer and remove the `?url` parameter from the address bar.

Notes:
- Many sites block embedding via HTTP headers like `X-Frame-Options` or `Content-Security-Policy`. If a page appears blank or fails to load, use the provided “Open in new tab” link.
- Only `http` and `https` URLs are accepted. If you omit the protocol (e.g., `example.com`), the app assumes `https://`.

## Code Explanation
The entire application is contained in `index.html`:

- Structure:
  - Header: App title and brief instructions.
  - Form controls: URL input and three buttons (Preview, Copy Link, Clear).
  - Status bars: One above the viewer (live status/error messaging) and one below (new-tab fallback).
  - Viewer: An `iframe` used to embed the target page.

- Styling:
  - Red theme implemented via CSS custom properties and gradients.
  - Minimal, accessible, and responsive layout.
  - No external fonts or CSS frameworks.

- JavaScript:
  - Query handling: On load, the app reads `?url=...`, normalizes and validates it, and auto-previews when valid.
  - Normalization: If the user omits a scheme, `https://` is assumed. Only `http(s)` URLs are accepted.
  - History updates: Uses `history.replaceState` to keep the address bar in sync with the current URL without reloading.
  - Iframe loading:
    - Sets `iframe.src` to the provided URL.
    - Shows a status badge (“Loading”, “Loaded”, or “Error”).
    - Displays a fallback message if the content doesn’t appear to load within a short timeout (some sites block embedding).
  - Clipboard:
    - “Copy Link” writes a shareable URL (including `?url`) to the clipboard using the Clipboard API, with a `document.execCommand` fallback.

- Security considerations:
  - The iframe uses `sandbox` with permissive flags to support more embedded sites while maintaining isolation from the host app.
  - `referrerpolicy="no-referrer"` reduces leaked referrer information to the embedded page.
  - Links to external sites use `rel="noopener noreferrer"`.

## License
MIT License

Copyright (c) 2025

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.