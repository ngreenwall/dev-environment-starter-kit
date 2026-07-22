# Worklog

## Now / Next
- [ ] Upload `social-preview.png` (repo root, untracked) to GitHub Settings > General > Social preview
- [ ] Decide whether to eventually commit `social-preview.png` into the repo, currently kept local-only by request

### Ideas

## Session notes

2026-07-22 | Shipped: added `docs/github-setup.md` (new optional guide: verifying Git via Xcode CLT, creating a GitHub account, installing/using `gh auth login` for HTTPS auth, setting `git config --global user.name/user.email`, first `gh repo clone`, first `git add/commit/push`); linked it from README's "Going further" section; propagated the same initial-setup gap fix to the Obsidian `GitHub.md` note (added an "Initial Setup (One Time per Computer)" section covering the same four steps, placed before "How to Create a New Repo"). | Next: none, session complete. | Blockers: none

2026-07-22 | Shipped: created repo with README.md (Homebrew/fnm/Node.js/npm/Python/Claude Code walkthrough), CLAUDE.md, LICENSE (MIT), zsh/dev-tools-additions.txt, zsh/recommended-zshrc.txt (full drop-in .zshrc with aliases/functions/prompt), docs/customizing-zshrc.md, docs/api-key-management.md; cross-linked with terminal-starter-kit in both READMEs; ran a 3-agent review (technical accuracy, internal consistency, beginner clarity) and fixed all findings, incl. removing a redundant `fnm default lts-latest` step, fixing a contradictory "does nothing on its own" comment on the npm Global Toolbox block, and adding missing plain-language definitions in docs/api-key-management.md; propagated the fnm/Claude-Code-install findings back to the source Obsidian notes (fnm, Node.js & npm.md; zshrc.md; Mac Development Environment Setup.md); pushed to `github.com/ngreenwall/dev-environment-starter-kit`; set GitHub repo description and topics for discoverability; designed a 1280x640 social preview card in Figma (https://www.figma.com/design/1uCHn2pButmN0yxub5eboC) matching the Claude-inspired palette, verified against GitHub's 40pt safe-zone template, exported to `social-preview.png` at repo root, kept untracked by request, not committed. | Next: upload social preview image to GitHub settings. | Blockers: none
