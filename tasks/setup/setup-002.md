---
id: setup-002
title: Initialize Git repository and push to GitHub
status: done
priority: critical
tags:
- setup
dependencies:
- frontend-002
assignee: developer
created: 2026-03-04T09:00:27.886769823Z
estimate: 30m
complexity: 2
area: setup
---

# Initialize Git repository and push to GitHub

## Causation Chain
> Frontend complete (frontend-002) → git init → GitHub repo → GitHub Actions
can reference the repo. No repo = no CI/CD pipeline (deployment-001).

## Pre-flight Checks
- [ ] Confirm frontend-002 session handoff is complete
- [ ] All files present: `index (1).html`, `assets/arc-mark-white.svg`, `assets/eliran-sabag.jpg`
- [ ] `gh auth status` — confirm GitHub CLI is authenticated
- [ ] Decide repo name: `ARC-ceo` or `arc.ceo` (GitHub Pages uses `<user>.github.io` for root)

## Context
This is a static site (`index.html` + assets) for arc.ceo.
Hosting strategy: **GitHub Pages** with custom domain.
- Free tier, automatic HTTPS via Let's Encrypt
- GitHub Actions deploys on every push to `main`
- Custom domain set in repo Settings → Pages → Custom domain: `arc.ceo`
- GoDaddy DNS configured in deployment-002

Repo visibility: **Public** (required for free GitHub Pages)
Repo name convention for GitHub Pages: if repo is named `<username>.github.io`,
it serves at that URL by default. Otherwise any repo can serve to a custom domain.
Recommended: name it `ARC-ceo` and set custom domain to `arc.ceo`.

## Tasks
- [ ] `git init` in `/data/git/ARC.ceo/`
- [ ] Create `.gitignore` (exclude: `*.pdf`, `ARC BRANDING.pdf`, `tasks/setup/001-project-setup.md`)
- [ ] Rename `index (1).html` → `index.html`
- [ ] Create `README.md` with one-line description
- [ ] `git add` all tracked files and make initial commit
- [ ] `gh repo create eliransabag/ARC-ceo --public --source=. --remote=origin --push`
  (adjust GitHub username as needed)
- [ ] Confirm repo is live on GitHub

## Acceptance Criteria
- [ ] `git log --oneline` shows initial commit
- [ ] GitHub repo exists and is publicly accessible
- [ ] `index.html` is at repo root
- [ ] `assets/` directory with SVG and photo is committed

## Notes
- **Do not commit `ARC BRANDING.pdf`** — it's a large binary brand asset.
  It should be stored separately (Google Drive, Notion, etc.).
- The `.taskguard/` directory and `tasks/` directory SHOULD be committed
  (they are project management artifacts, not secrets).
- GitHub username: confirm with `gh api user --jq .login`

---
**Session Handoff** (fill when done):
- Changed: all project files now in git, pushed to GitHub
- Causality: GitHub repo URL needed by deployment-001 for Actions configuration
- Verify: `git remote -v` shows origin pointing to GitHub
- Next: deployment-001 — create `.github/workflows/deploy.yml`