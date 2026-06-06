# Decant

**Pour AI output into any app.**

Decant is a browser-based formatting tool that takes AI-generated text, terminal output, or raw markdown and reformats it for seamless pasting into the app you're actually using — Word, Notion, Slack, email, and more.

No install, no dependencies, no build step. **[Try it live →](https://mattwagstaff.github.io/decant)**

---

## The Problem

AI responses look great in the chat interface. Paste them into Google Docs or Slack, and the formatting breaks — asterisks appear as literal characters, code blocks collapse into a wall of text, tables turn into garbled pipes-and-dashes. Every app has its own formatting expectations, and copy-paste rarely meets them.

Decant bridges that gap.

---

## Features

- **11 target app formats** — Microsoft Word, Google Docs, Apple Pages, Apple Notes, Notion, Slack, Email, Linear, Confluence, Markdown, and Plain Text
- **Automatic format detection** — identifies headers, tables, code blocks, lists, checkboxes, blockquotes, ANSI terminal codes, and box-drawing characters
- **Rich clipboard copy** — writes both HTML and plain text to the clipboard so apps that support rich pasting get full formatting
- **Box-drawing table conversion** — converts CLI-style tables (using `─`, `│`, `┼` characters) into clean markdown tables
- **ANSI code stripping** — removes terminal color/style escape codes while preserving the text structure
- **Live preview** — formatted output updates in real time as you type or paste
- **Live stats** — word count, character count, and detected input format shown in the footer
- **Keyboard shortcuts** — `Ctrl+K` to clear, `Ctrl+Enter` to copy
- **Drag & drop** — drop a text file directly onto the input area

---

## Supported Output Formats

| App | Format |
|---|---|
| Microsoft Word | HTML with inline styles |
| Google Docs | Formatted HTML |
| Apple Pages | HTML |
| Apple Notes | HTML |
| Notion | HTML |
| Slack | Slack `mrkdwn` syntax |
| Email (Gmail / Outlook / Apple Mail) | HTML |
| Linear | Clean Markdown |
| Confluence | HTML |
| Markdown | Normalized Markdown |
| Plain Text | All formatting stripped |

---

## Usage

### Online

**[decant — mattwagstaff.github.io/decant](https://mattwagstaff.github.io/decant)**

### Local

```bash
git clone https://github.com/mattwagstaff/decant.git
cd decant
open index.html        # macOS
# or
python3 -m http.server 8000   # then visit http://localhost:8000
```

No `npm install`. No build step. It just works.

### Workflow

1. Paste your AI output or markdown into the left panel
2. Select your target app from the dropdown
3. Click **Copy** (or press `Ctrl+Enter`)
4. Paste into your app

---

## How It Works

Decant is a single HTML file (~32 KB). The entire application — markup, styles, and logic — lives in `index.html`.

**Detection pipeline**: On each input change, Decant scans the text for known patterns (markdown syntax, ANSI codes, box-drawing characters) to identify the input format and surface it in the footer.

**Conversion pipeline**: Based on the selected target app, the input is routed through one of several formatters:

- **HTML formatter** (Word, Docs, Pages, Notes, Notion, Email, Confluence) — parses markdown with [marked.js](https://marked.js.org/) and applies inline styles for maximum compatibility
- **Slack formatter** — converts markdown to Slack's `mrkdwn` syntax (bold, italic, code, blockquotes, bullet lists)
- **Plain text formatter** — strips all formatting, converts tables to aligned text columns
- **Markdown normalizer** — cleans and normalizes markdown syntax without changing structure

**Clipboard strategy**: Uses the modern `ClipboardItem` API to write both `text/html` and `text/plain` simultaneously, so rich-text apps get formatted HTML and plain-text apps get clean fallback content. Falls back to `execCommand('copy')` for older browsers.

---

## Project Structure

```
decant/
└── index.html    # Everything — markup, CSS, JavaScript
```

There are no other files required. CDN-loaded dependencies:

- [marked.js v9](https://cdn.jsdelivr.net/npm/marked@9) — markdown parsing
- [Inter](https://fonts.google.com/specimen/Inter) — UI font (Google Fonts)
- [JetBrains Mono](https://fonts.google.com/specimen/JetBrains+Mono) — monospace font (Google Fonts)

---

## Browser Support

Any modern browser with support for the [Clipboard API](https://developer.mozilla.org/en-US/docs/Web/API/Clipboard_API). Chrome, Firefox, Safari, and Edge all work. The app falls back gracefully when `ClipboardItem` is unavailable.

---

## Contributing

The entire app is one file. To make changes:

1. Fork the repo
2. Edit `index.html`
3. Open it in a browser to test
4. Submit a pull request

There's no build process to set up or toolchain to install.

---

## License

MIT
