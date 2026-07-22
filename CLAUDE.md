# CLAUDE.md

Repo-specific context for working in this project. See `~/.claude/CLAUDE.md` for global preferences.

## What this repo is

A small, public, beginner-facing starter kit: a step-by-step guide to installing the core Mac dev tools (Xcode Command Line Tools, Homebrew, fnm, Node.js, npm, Python, Claude Code). Companion to `terminal-starter-kit` (that one covers making the terminal look better; this one covers what to install). Audience is non-developers, not just engineers browsing GitHub.

## Conventions established in this repo

**File naming**: config files meant to be copy-pasted use a `.txt` extension (`zsh/dev-tools-additions.txt`) instead of their real dotfile names, so they open in a text editor on double-click instead of prompting "no application set to open this file." Matches the convention from `terminal-starter-kit`.

**Instruction consistency**: comments inside `dev-tools-additions.txt` must match the steps described in `README.md` exactly, same as `terminal-starter-kit` enforces between its config file headers and README.

**Writing style for docs**: `README.md` is written for non-developers. No assumed jargon, explain what each thing is and why it matters, and include a plain-language "what this does" line after every command block.

**Scope discipline**: this repo covers Homebrew, fnm, Node.js, npm, Python, and Claude Code, in that install order (each step depends on the one before it). Don't expand into unrelated tools (Google Cloud CLI, Docker, databases, etc.) unless explicitly asked.

**Cross-linking**: this repo and `terminal-starter-kit` link to each other in their READMEs. If either repo's README structure changes significantly, check the other still links correctly.
