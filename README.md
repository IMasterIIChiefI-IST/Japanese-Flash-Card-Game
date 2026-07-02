# Japanese Flashcards + Kana Ninja

A browser-based Japanese study app with two main modes:

- **Vocabulary Drill** — practice Japanese vocabulary from an Excel spreadsheet, optionally backed by an offline Japanese-English dictionary.
- **Kana Ninja** — type romaji quickly as kana slide across the screen.

It also includes a **Grammar Guide** tab (Tae Kim's *Guide to Japanese*, downloadable for fully offline reading) and a standalone dictionary lookup / kana reference tab.

The app is a single HTML file designed to run locally in a browser, with optional Excel vocabulary loading, optional offline dictionary support, and file-based progress backup.

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
  - Romaji → English
  - English → Romaji
  - Kana → Romaji
  - Kanji + kana → Romaji
- Tracks learning progress with statuses such as new, learning, known, and failing.
- Includes an easy mode that can show romaji hints for selected prompt types.
- Lets you browse known words, searching by **English or romaji** (kana/kanji are intentionally excluded from this search so you can look words up by what you actually remember).
- Lets you add new vocabulary manually, with optional dictionary-assisted lookup (see below).
- Auto-fills the source field for new entries using:

```text
DDMMYYYY - game version
```

#### Multiple English meanings, handled properly

Words with more than one English meaning (e.g. `cow / cattle`) are supported end-to-end:

- When English is the **prompt**, the app shows one meaning at a time, chosen at random from the full list — so repeated study cycles through all of them instead of always showing the first.
- When English is the **answer**, typing any single valid meaning is accepted, and so is typing a **combination** of meanings joined by `/`, a comma, `&`, or "and" (e.g. `cow / cattle` or `cow and cattle`). Every piece you type has to match a real meaning for that card — random extra words still count as wrong.

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

**Add meanings from dictionary** — with a dictionary loaded (see below), this button scans every word in your spreadsheet by its kana reading, looks it up, and appends any English meanings you're missing. It's homophone-aware: if two different words share a reading but have different kanji (e.g. 日本 "Japan" vs. 二本 "two long thin objects", both にほん), it only merges meanings from the entry that actually matches your word's kanji, and skips ambiguous cases entirely rather than guessing.

### Optional Offline Dictionary (JMdict)

The app can optionally load [JMdict](https://github.com/scriptin/jmdict-simplified) — a free, open-license Japanese-English dictionary — to assist with adding and completing vocabulary. This is entirely optional; the app works fully without it.

- **⬇ Download dictionary** — fetches the current JMdict "common words" release directly from GitHub, offers to save it next to the app via your browser's save dialog, and loads it into the current session immediately. No manual download required in the happy path.
- **Load dictionary file** — manually load a `.json` or `.json.zip` JMdict-simplified file (handy if the automatic download is blocked by your browser, or if you want the full dictionary instead of the common-words subset).
- **Try same-folder dictionary** — looks for `jmdict-eng.json`, `jmdict-eng.json.zip`, `jmdict-eng-common.json`, or `jmdict-eng-common.json.zip` next to the app, same pattern as the Excel auto-load. Tried automatically on every page load.
- **Unload dictionary** — clears it from memory.
- Everything runs locally in the browser once downloaded — no data is ever sent anywhere, and the game works fully offline after the dictionary file is in place.

With a dictionary loaded, the "Add a new word" form gains a **Look up** search box: type an English word, pick a result, and it auto-fills Kana, Kana script, Kanji, Kanji + kana, and Romaji (romaji is generated from the kana with a Hepburn-style converter — good for most words, but a handful of irregular readings may need a manual tweak). You can edit any field before saving.

### Grammar Guide (offline PDF)

The **Grammar Guide** tab embeds Tae Kim's free *Guide to Japanese* — particles, verb conjugation, sentence structure, and more — as a searchable PDF, using the same "download once, then fully offline" pattern as the dictionary:

- **⬇ Download guide for offline use** — fetches the PDF once and saves it in the browser's local IndexedDB storage. Nothing is fetched from the network automatically; the guide only downloads when you click this button.
- **📂 Load PDF file** — manually import a PDF you already have on disk (handy if the automatic download is blocked by CORS — see [Notes About Browser Limitations](#notes-about-browser-limitations)).
- **🗑 Remove offline copy** — clears the stored PDF from this browser.
- **↗ Open live copy online** — optional link to view the guide directly on [guidetojapanese.org](https://www.guidetojapanese.org/) without downloading; requires an internet connection.

Once downloaded or loaded, the guide is stored locally and reopens instantly on future visits with **no network connection required at all** — including when the app is opened as a plain `file://` page with no server. The search box jumps the embedded viewer to the first match of a term (e.g. "particles" or "te-form"); use the viewer's own arrows or `Ctrl/Cmd+F` to step through the rest.

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
├── japanese_flashcards_excel_loader.html
├── japanese_vocabulary_by_category.xlsx
├── japanese_flashcards_progress.json
├── jmdict-eng-common.json.zip   (optional — enables offline dictionary lookup)
└── README.md
```

The app looks for `japanese_vocabulary_by_category.xlsx` by name (via the "Try same-folder Excel" button), so keep that filename as-is. The same applies to the optional dictionary file — see the filenames listed above. The Grammar Guide PDF is *not* loaded from a same-folder file — it's stored inside the browser itself (IndexedDB) after you click **Download guide for offline use** or **Load PDF file**, so no extra file needs to sit next to the app for it. You can rename the HTML file to `index.html` if you want GitHub Pages or a simple static server to open it more easily — just update any links/shortcuts accordingly.

## Running Locally

### Option 1: Open directly

Double-click the HTML file and open it in your browser.

This is the simplest method, but some browser features may be limited, especially automatic same-folder Excel/dictionary loading.

### Option 2: Use a local server

From the project folder, run:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000/japanese_flashcards_excel_loader.html
```

This usually gives better results for loading files from the same folder.

## Using the App

1. Open the HTML file in your browser.
2. Load your Excel vocabulary file.
3. (Optional) Download or load a JMdict dictionary file to enable lookup-assisted word entry and the "Add meanings from dictionary" button.
4. (Optional) Visit the Grammar Guide tab and click **Download guide for offline use** (or **Load PDF file**) to save Tae Kim's guide for offline reading.
5. Choose categories and flashcard modes.
6. Start practicing.
7. Use the progress save/load tools to preserve your study history.
8. Use Kana Ninja for kana speed practice.

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

This also affects the dictionary download button: some browsers/hosts block in-page downloads from GitHub's release host due to CORS restrictions. When that happens, the app automatically falls back to a normal browser download (the file lands in your Downloads folder), and you'll need to move it next to the app manually.

The Grammar Guide download button can hit the same kind of CORS restriction when fetching from guidetojapanese.org. If the automatic download fails, use **Open live copy online** to view/save the PDF manually, then use **Load PDF file** in the Grammar Guide tab to store it offline — it only has to be done once.

## Tech

- HTML
- CSS
- JavaScript
- Excel parsing/export through browser-side spreadsheet handling ([SheetJS](https://sheetjs.com/))
- Client-side zip decompression for dictionary files ([fflate](https://github.com/101arrowz/fflate))
- Optional dictionary data from [JMdict-simplified](https://github.com/scriptin/jmdict-simplified) (JMdict is a free, Creative Commons-licensed Japanese-English dictionary)
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
- Duplicate/conflict checker for the vocabulary spreadsheet (catch homophone mix-ups automatically)
- Bulk word import from the dictionary (add many new entries from a pasted English word list at once)
- Spaced repetition scheduling instead of pure random card selection
- Typo-tolerant answer checking for kana/romaji

## License

  GNU AFFERO GENERAL PUBLIC LICENSE 3.0
