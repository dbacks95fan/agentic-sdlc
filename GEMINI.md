<!--
Pointer file for Gemini CLI. Gemini CLI reads GEMINI.md by default and supports @import of other Markdown files.

Two ways to wire the canonical file:

1. This pointer: copy GEMINI.md AND AGENTS.md to the repo root (or to ~/.gemini/ for global use) and keep the import below. Run `/memory refresh` after edits.

2. No pointer: in .gemini/settings.json set { "context": { "fileName": ["AGENTS.md", "GEMINI.md"] } } and Gemini CLI will load AGENTS.md directly. Delete this file if you do that.

Import paths resolve relative to this file's location.
-->

@AGENTS.md

# Gemini CLI notes

<!-- Gemini-specific rules only. Appended after the canonical file. -->
