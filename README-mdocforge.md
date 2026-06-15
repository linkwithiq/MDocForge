# MDocForge

**MDocForge** is a single-file, browser-based document editor that lets you write in **Markdown or HTML**, see a live visual preview as you type, and export the result to **PDF, Word (.docx), HTML, Markdown, PNG, or plain text** — all without a server or sign-up. Everything runs locally in your browser tab.

Open `mdocforge.html` in any modern browser to use it. There is nothing to install.

---

## What it is

MDocForge is a "block-based" document builder, similar in spirit to tools like Notion or the WordPress block editor, but built around Markdown. Your document is represented internally as a list of **blocks** (headings, paragraphs, lists, tables, images, etc.). These blocks can be:

- Typed/pasted as raw Markdown or HTML in the editor pane
- Edited directly in the live preview (click on text and type)
- Added by dragging ready-made block types from the left sidebar

Whichever way you create content, the editor, the preview, and the underlying block model all stay in sync.

---

## Key Features

### Two editing modes
- **Markdown mode** — write standard Markdown (headings `#`, lists, tables, blockquotes, code fences, images, links, etc.) and watch it render instantly.
- **HTML mode** — write raw HTML and the editor parses it into the same block model.

### Three layout views
- **Split** — editor and preview side by side (default).
- **Editor** — full-width text editor only.
- **Preview** — full-width rendered document only.

### Block palette (drag-and-drop)
A sidebar of ready-made content blocks you can drag onto the page or click to append:
- Heading 1 / 2 / 3
- Paragraph
- Quote
- Bullet list
- Numbered list
- Table
- Image
- Code block
- Divider (horizontal rule)
- Link

Dropping a block above or below an existing block inserts it at that exact position.

### In-place visual editing
Every block on the canvas can be edited directly:
- Click any heading, paragraph, quote, or list item to edit its text inline.
- Click table cells to edit them directly.
- Click an image to change its source URL.
- Edit code blocks directly in the rendered `<pre><code>` element.

### Block toolbar
Hover (or select) any block to reveal a small floating toolbar with:
- **↑ / ↓** — move the block up or down
- **⧉** — duplicate the block
- **✕** — delete the block

A drag handle (`⠿`) on the left of each block lets you reorder content by dragging it to a new position.

### Live document preview
The preview pane renders your content as a styled A4-width page (760px), using a clean serif body font with sans-serif headings — designed to look like a real document or report, not a plain text dump.

### Export options
A single **Export** menu lets you save your document as:

| Format | Description |
|---|---|
| **PDF** | A4 page-by-page export. The exporter intelligently paginates at block boundaries so headings, paragraphs, tables, etc. are never sliced in half across a page break. |
| **Word (.docx)** | Converts your document into a real Word file with proper headings, paragraphs, bold/italic text, bullet/numbered lists, tables, and blockquotes (via the `docx.js` library). |
| **HTML website** | A complete, self-contained, styled `.html` file you can host or share as a standalone webpage. |
| **Markdown (.md)** | The raw Markdown source of your document. |
| **Image (.png)** | A high-resolution (2×) screenshot of the rendered document. |
| **Plain text (.txt)** | All text content with formatting stripped. |

Every export prompts a "Save file as" dialog so you can choose your own filename before downloading.

### Sample document
The **Load sample** button fills the editor with a ready-made "Quarterly Report" example (headings, highlights list, a quote, a metrics table, numbered next steps, and a link) so you can immediately see all block types and export formats in action.

### Live counters
A status bar at the bottom shows the current mode (Markdown/HTML), live word count, and character count.

### Responsive design
On smaller screens, the block palette hides and the editor/preview panes stack vertically instead of side-by-side.

---

## How it works (under the hood)

MDocForge is a **single static HTML file** with embedded CSS and JavaScript. It loads a few small libraries from a public CDN to handle conversions:

- **marked.js** — converts Markdown text to HTML for the live preview.
- **html2canvas** — renders the document as a canvas image for PNG/PDF export.
- **jsPDF** — assembles the canvas slices into a multi-page PDF.
- **FileSaver.js** — triggers the browser's file-save/download dialog.
- **docx.js** — builds a real `.docx` (Word) file from the document's HTML structure.

### The data flow

1. **Parsing**: Whatever you type in the editor (Markdown or HTML) is parsed into an array of **block objects** — e.g. `{type: 'h1', content: 'Title'}` or `{type: 'table', headers: [...], rows: [...]}`.
2. **Rendering**: Each block object is rendered into the preview canvas as a real, editable HTML element (an `<h1>`, `<p>`, `<table>`, etc.), wrapped with a hover toolbar and drag handle.
3. **Editing sync**: Any edit you make — typing in a block, reordering blocks, changing an image URL, adding/removing list items — updates the block array, which is then converted back into Markdown or HTML and written back into the editor textarea, keeping both views in sync.
4. **Exporting**: When you choose an export format, the block array is converted into clean HTML (`blocksToHtml()`), then handed to the appropriate library:
   - For **PDF/PNG**, an off-screen styled copy of the document is rendered and captured with `html2canvas`. For PDF, the page is sliced into print-page-sized chunks at safe break points (never mid-element) and assembled with `jsPDF`.
   - For **DOCX**, the HTML is walked element-by-element and converted into corresponding `docx.js` paragraphs, headings, lists, and tables.
   - For **HTML/Markdown/Text**, the content is simply serialized and saved directly.

Because everything happens client-side, **no document content is ever sent to a server** — your files stay entirely on your device.

---

## Quick start

1. Open `mdocforge.html` in a browser (Chrome, Edge, Firefox, or Safari).
2. Click **Load sample** to see an example document, or start typing your own Markdown/HTML in the left pane.
3. Drag blocks from the left sidebar onto the page to add new sections, tables, images, etc.
4. Click any text directly in the preview to edit it in place.
5. When ready, click **Export** and choose your desired format (PDF, DOCX, HTML, Markdown, PNG, or TXT), then confirm the filename to download.
