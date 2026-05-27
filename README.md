# File Integrity Checker

A portable, browser-based file integrity verification tool. No installation, no server — just open `index.html` in any modern browser.

## Features

- **Compare Folders** — Compare two folders side-by-side with SHA-256 hashing. Detects identical, modified, missing, extra, and relocated files.
- **Compare Files** — Compare two individual files byte-by-byte via SHA-256.
- **Generate Checksums** — Generate a `sha256sum`-compatible checksum file for any folder.
- **Verify Checksums** — Verify files against a previously generated checksum file.
- **Flexible Matching** — Cross-machine verification that ignores directory structure differences (matches by filename or content hash).
- **Smart Caching** — Uses `localStorage` to cache hashes; unchanged files are verified instantly on subsequent runs.
- **Streaming Hash** — Pure JavaScript SHA-256 implementation processes large files (4K video, etc.) in chunks without loading everything into memory.

## Usage

1. Open `index.html` in Chrome, Edge, Firefox, or Safari.
2. Select the appropriate tab for your task.
3. Use the "Browse" buttons to select files/folders.
4. Click "Start" and wait for results.
5. Optionally save the report for record keeping.

## Cross-Machine Workflow

When verifying files transferred between machines (e.g., Mac → Windows):

1. On the **source machine**: use the "Generate Checksums" tab to create a checksum file and save it.
2. Transfer the checksum file alongside your data.
3. On the **target machine**: use the "Verify Checksums" tab, load the checksum file, select the target folder, enable **Flexible matching** if directory structures differ, then start verification.

## Technical Details

- **Zero dependencies** — Single HTML file with embedded CSS and JavaScript.
- **No server** — Everything runs locally in the browser. Files are never uploaded anywhere.
- **SHA-256** — Streaming implementation handles files of any size.
- **Cache** — Stored in browser `localStorage` keyed by folder name, relative path, file size, and last modification time.

## Compatibility

Works on any OS with a modern browser:
- macOS (Chrome, Safari, Edge, Firefox)
- Windows (Chrome, Edge, Firefox)
- Linux (Chrome, Firefox)

## License

MIT
