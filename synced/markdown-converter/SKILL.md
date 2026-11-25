---
name: "markdown-converter"
description: "MANDATORY whenever the user attaches or uploads any file (PDF, DOCX, PPTX, XLSX, CSV, HTML, EPUB, etc.): convert it to clean, token-light Markdown with markitdown before reading it."
---

# Markdown Converter (markitdown)

The user wants every attached file converted to Markdown with Microsoft MarkItDown before you work with it. Do this first, every time a file is attached, before answering questions about it.

## Step 1: Convert

For each attached file, write the Markdown to a file. Don't print the whole output into the conversation.

```bash
mkdir -p /tmp/md
# Prefer an installed markitdown; fall back to uvx (no install needed)
if command -v markitdown >/dev/null; then MD="markitdown"; else MD="uvx markitdown"; fi
$MD "<path/to/attached-file>" -o "/tmp/md/<name>.md"
```

- Never pass `--keep-data-uris`. The default truncates base64 images, which saves a lot of tokens.
- Supported: PDF, DOCX, PPTX, XLSX/XLS, CSV, JSON, XML, HTML, EPUB, ZIP (each file inside is converted), audio (transcription if available), images (metadata only).

## Step 2: Read economically

1. Check the size first: `wc -c -l /tmp/md/<name>.md`.
2. If it's under ~30 KB, read it whole with the Read tool.
3. If it's larger, list the headings first (`grep -n '^#' /tmp/md/<name>.md`), then read only the sections the task needs, using Read with offset/limit or `grep -n -A` for specific terms.

## Step 3: Check quality, then fall back if needed

MarkItDown is weak on some PDFs. Fall back when any of these happen:

- The output is empty or nearly empty. This usually means a scanned PDF with no text layer.
- Tables come out as run-together text, or headings are lost, and the user needs that structure.

Fallback order:
1. `pdftotext -layout file.pdf -` (poppler) for born-digital PDFs where layout matters.
2. Docling, if available: `docling file.pdf --to md --image-export-mode placeholder --output /tmp/md`. Always pass `--image-export-mode placeholder`; its default embeds base64 images.
3. For scanned pages, OCR (e.g. `pytesseract` on rendered pages), or look at the page images directly.

Tell the user in one line if you had to fall back, and why.

## Notes

- Images the user attaches are usually already visible to you. Still run markitdown for metadata if asked, but rely on your own vision for the content.
- Work on a copy. Never modify the user's original upload.
- If the user asks for the Markdown itself, send the `.md` file as the deliverable.