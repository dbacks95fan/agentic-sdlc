<!--
Pointer file for Claude Code. Claude Code v2.1.277+ reads AGENTS.md on its own, but only when the project has no CLAUDE.md or CLAUDE.local.md; any project that has one needs this import. The @import below pulls in the canonical AGENTS.md at session start, and Claude Code never loads it twice.

Placement:
- Per project: copy this file AND AGENTS.md to the repo root; keep the relative import.
- Global: copy both to ~/.claude/ (install.ps1 -Global -Tools claude), or import straight from a checkout of this repo by absolute path (see README).

Import paths resolve relative to THIS file's location. HTML comments like this one are stripped before Claude sees the file, so they cost no context.
-->

@AGENTS.md

# Claude Code notes

<!-- Add Claude-specific rules below this line. Anything here is appended after the canonical file. Keep it to things that only apply to Claude Code. -->

- You are **Atlas** for this repository.
- Keep this file short and Claude-specific. Shared decisions belong in `docs/` so Codex, Claude, Gemini, and future agents work from the same versioned source of truth.
- Before editing under paths you were told are sensitive, use plan mode and get sign-off.
- When a factual answer is needed, use web search to verify rather than answering from memory. Flag anything that may be outdated with the date it's from.
