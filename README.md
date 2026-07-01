# Japanese Flashcards + Kana Ninja

A browser-based Japanese study app with two main modes:

- **Vocabulary Drill** — practice Japanese vocabulary from an Excel spreadsheet.
- **Kana Ninja** — type romaji quickly as kana slide across the screen.

The app is a single HTML file designed to run locally in a browser, with optional Excel vocabulary loading and file-based progress backup.

## Disclaimer

This project was **mostly vibe coded**. It works for the intended personal study workflow, but it should be treated as a practical learning tool rather than polished production software. Expect rough edges, browser quirks, and code that may need cleanup before serious reuse.

## Features

### Vocabulary Drill

- Loads vocabulary from an Excel file instead of relying on hardcoded words.
- Supports category selection.
- Supports multiple flashcard directions, including:
  - English → Kana
  - Kana → English
  - Kanji / Japanese form → English
  - Kana → Romaji
  - Kanji + kana → Romaji
- Tracks learning progress with statuses such as new, learning, known, and failing.
- Includes an easy mode that can show romaji hints for selected prompt types.
- Lets you browse known words.
- Lets you add new vocabulary manually.
- Auto-fills the source field for new entries using:

```text
DDMMYYYY - game version
```

### Excel Vocabulary Support

The app expects an Excel workbook with columns like:

```text
ID
Category
English
Kana
Kana script
Kanji / standard form
Kanji + kana form
Romaji pronunciation
Notes
Source(s)
```

You can load the spreadsheet manually from the app interface. When opened from a normal local file, browsers may block automatic same-folder loading, so manual loading is the safest option.

### Progress Saving

The app supports several ways to preserve progress:

- Browser/local save when available.
- Manual progress export/import as a JSON file.
- Optional folder connection using the browser File System Access API.
- Auto-saving to a progress file after connecting a save folder.

The progress file is intended to be named:

```text
japanese_flashcards_progress.json
```

Browser support varies. Chromium-based browsers usually work best for folder-based saving.

### Kana Ninja

Kana Ninja is a typing game for practicing kana recognition.

Options include:

- Hiragana on/off
- Katakana on/off
- Base kana on/off
- Dakuten + handakuten on/off
- Yōon on/off
- Sokuon on/off
- Speed control
- Lane count control
- Max kana per row control
- Optional romaji hints
- Visible marker showing where romaji hints begin
- Infinite lives mode

## Recommended Files

A typical repo layout could look like this:

```text
.
├── index.html
├── japanese_vocabulary_by_category.xlsx
├── japanese_flashcards_progress.json
└── README.md
```

You can rename the HTML file to `index.html` if you want GitHub Pages or a simple static server to open it more easily.

## Running Locally

### Option 1: Open directly

Double-click the HTML file and open it in your browser.

This is the simplest method, but some browser features may be limited, especially automatic same-folder Excel loading.

### Option 2: Use a local server

From the project folder, run:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

This usually gives better results for loading files from the same folder.

## Using the App

1. Open the HTML file in your browser.
2. Load your Excel vocabulary file.
3. Choose categories and flashcard modes.
4. Start practicing.
5. Use the progress save/load tools to preserve your study history.
6. Use Kana Ninja for kana speed practice.

## Saving Progress

For best results:

1. Click **Connect save folder**.
2. Choose the same folder where the app is stored.
3. Let the app create or update `japanese_flashcards_progress.json`.
4. Keep that JSON file with the repo or back it up separately.

If folder saving is not available in your browser, use the manual **Save progress file** and **Load progress file** buttons.

## Notes About Browser Limitations

Because this is a local browser app, it cannot always write files automatically without permission. This is a browser security restriction, not an app bug.

Some features may behave differently depending on whether the app is opened as:

```text
file://...
```

or served from:

```text
http://localhost:8000
```

## Tech

- HTML
- CSS
- JavaScript
- Excel parsing/export through browser-side spreadsheet handling
- No backend required

## Future Ideas

Possible improvements:

- Cleaner code organization
- Dedicated settings export/import
- More study modes
- Better mobile layout
- More detailed progress analytics
- GitHub Pages setup
- Automated tests for vocabulary parsing and answer checking

## License

No license has been selected yet. Add one before distributing or accepting contributions.
