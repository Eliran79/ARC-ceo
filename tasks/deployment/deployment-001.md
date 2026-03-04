---
id: deployment-001
title: GitHub Actions deploy to GitHub Pages
status: done
priority: critical
tags:
- deployment
dependencies:
- setup-002
assignee: developer
created: 2026-03-04T09:05:55.212624164Z
estimate: 2h
complexity: 5
area: deployment
---

# GitHub Actions deploy to GitHub Pages

## Causation Chain
> Repo on GitHub (setup-002) → GitHub Actions workflow created → pushes to
`gh-pages` branch → GitHub Pages serves the site → GoDaddy DNS (deployment-002)
points arc.ceo to GitHub Pages IP addresses.

## Pre-flight Checks
- [ ] Confirm `git remote -v` shows GitHub origin (setup-002 handoff)
- [ ] GitHub repo is public (required for free Pages)
- [ ] `gh api user --jq .login` — note your GitHub username

## Context
This is a **static site** (pure HTML + assets, no build step).
GitHub Actions workflow: on push to `main` → copy files to `gh-pages` branch.

**Chosen approach**: GitHub Pages (not AWS) because:
- Zero cost for static sites
- Automatic HTTPS / Let's Encrypt via GitHub
- GitHub Actions native integration
- Custom domain support (arc.ceo) via CNAME file + GoDaddy DNS

**Workflow logic**: Since there's no build step, the workflow simply uses
`peaceiris/actions-gh-pages` to push `main` branch content to `gh-pages`.

## Tasks
- [ ] Create `.github/workflows/deploy.yml` with the following workflow:
  ```yaml
  name: Deploy to GitHub Pages
  on:
    push:
      branches: [main]
    workflow_dispatch:
  permissions:
    contents: write
  jobs:
    deploy:
      runs-on: ubuntu-latest
      steps:
        - uses: actions/checkout@v4
        - name: Deploy to GitHub Pages
          uses: peaceiris/actions-gh-pages@v4
          with:
            github_token: ${{ secrets.GITHUB_TOKEN }}
            publish_dir: .
            exclude_assets: '.github,tasks,.taskguard,*.pdf,AGENTIC_AI_TASKGUARD_GUIDE.md'
  ```
- [ ] Create `CNAME` file at repo root with content: `arc.ceo`
- [ ] Push to GitHub (`git add .github/CNAME && git commit && git push`)
- [ ] Go to GitHub repo Settings → Pages → Source: `gh-pages` branch, `/ (root)`
- [ ] Confirm Actions workflow runs and `gh-pages` branch is created
- [ ] Note the GitHub Pages URL (e.g. `https://eliransabag.github.io/ARC-ceo/`)

## Acceptance Criteria
- [ ] `.github/workflows/deploy.yml` exists in repo
- [ ] `CNAME` file contains `arc.ceo`
- [ ] GitHub Actions run succeeds (green checkmark)
- [ ] `gh-pages` branch contains `index.html` and `assets/`
- [ ] Temporary URL (`*.github.io`) returns the ARC site

## Notes
- `GITHUB_TOKEN` is automatically available — no secrets to configure
- The `exclude_assets` prevents brand PDF, tasks, and workflow files from
  being deployed to the public site
- `workflow_dispatch` allows manual re-runs from GitHub UI
- After DNS propagation (deployment-002), GitHub will auto-provision SSL cert

---
**Session Handoff** (fill when done):
- Changed: `.github/workflows/deploy.yml`, `CNAME`
- Causality: Push to main → Actions → gh-pages branch → Pages serves site
- Verify: Check Actions tab in GitHub repo, visit `*.github.io` URL
- Next: deployment-002 — configure GoDaddy to point arc.ceo here