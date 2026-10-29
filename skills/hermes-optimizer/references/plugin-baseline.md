# Plugin baseline

A stock install ships with no third-party plugins, which means none of these toolsets exist yet.
This file is the recommended baseline, plus the fit test that decides which ones a given user
should actually get.

## The fit test

Before proposing any plugin, ask:

1. Does the user's stated work actually need this capability?
2. Is the capability already available somewhere that is live - a bundled toolset, a configured
   MCP server, or a hosted model route? If yes, skip the plugin.
3. Does it add a toolset the user will plausibly call this month?
4. Does it need credentials the user has not configured? If so, name the credential up front.

If a plugin fails 1 or 2, do not propose it. A plugin nobody calls is prompt overhead and a
maintenance surface.

## Category 1 - agent runtime and context

These are the ones that change how the agent itself works. They earn their place on almost any
serious install.

| Plugin | What it buys | Notes |
|--------|--------------|-------|
| Context engine (lossless) | keeps every turn searchable instead of compacting it away; adds recall, search, and expansion tools | selected via the context-engine setting, not just installed. Disclosure: stores turns locally in plaintext, and optional semantic recall can send text to an embedding provider. Tell the user that before enabling. |
| Delegation (advanced) | sub-agent spawning with per-task isolation, so heavy work does not flood the main context | pairs with the delegation config block |
| Teammates | multi-agent coordination across profiles | only propose if the user actually runs parallel workstreams |
| Provider status | surfaces provider health and route state | useful when a mixed stack starts failing intermittently |
| Web search plus | multi-provider search and extraction with a quality report | useful when the built-in web toolset hits blocked or rate-limited pages |

Fit questions that select this category: "Do you run long sessions you need to search later?",
"Do you delegate work to sub-agents?", "Does the built-in web search fail on the sites you need?"

## Category 2 - documents and office

| Plugin | What it buys | Notes |
|--------|--------------|-------|
| Office desk | guarded Word, PowerPoint, and Excel editing with tracked changes and revertible receipts | needs LibreOffice installed for spreadsheet recalc. Dependency conflict warning: it must not share an environment with plain python-docx/python-pptx/openpyxl. |
| PDF surgeon | edit text inside existing PDFs | complements, does not replace, text extraction |
| Redline | review workflow for documents the user reads and marks up |  |

Fit questions: "Do you edit Word, PowerPoint, or Excel files and need tracked changes?", "Do you
need to change text inside a PDF rather than just read it?", "Do you review documents and send
them back with comments?"

## Category 3 - document and data reading

This is the base layer. Text extraction is what everything else stands on.

The key point from the anti-bloat test: **hosted vision does not replace structured text
extraction.** A vision route reads an image of a page. A text extractor gives you the actual
characters, in reading order, with structure. For PDFs and Markdown the extractor is the right
tool, and it is small.

Preferred order:

1. **Use what is already installed.** A Python with PyMuPDF handles born-digital PDF text
   extraction and Markdown reading with no new heavy dependency. Check for it before installing
   anything.
2. **Add an extractor only if step 1 is missing or insufficient.**
3. **Add OCR only if the user has scans.** And only after checking that the installed OCR is not
   a multi-gigabyte stack duplicating a live vision route. Scans at volume or a local-only
   requirement justify it; "it might be nice" does not.

Fit questions: "Which formats do you handle - born-digital PDFs, scans, Word, Markdown,
spreadsheets?", "Is there a set of scanned documents you need text out of?", "Does any of this
need to stay on the machine?"

## Reporting a rejected plugin

When you skip something the user asked about, say why in one line:

- "That duplicates the vision route you already have, so it would be a second copy to maintain."
- "Your context engine already does semantic recall, so a second embedding stack would fight it."
- "No box on this machine runs that platform, so the tools would never fire."

The user is paying in prompt overhead and maintenance for every plugin. Saying no with a reason is
part of the job.