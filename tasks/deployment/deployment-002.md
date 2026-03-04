---
id: deployment-002
title: Configure GoDaddy DNS for arc.ceo
status: done
priority: critical
tags:
- deployment
dependencies:
- deployment-001
assignee: developer
created: 2026-03-04T09:05:58.243452346Z
estimate: 30m
complexity: 3
area: deployment
---

# Configure GoDaddy DNS for arc.ceo

## Causation Chain
> GitHub Pages is live (deployment-001) → GoDaddy DNS updated → arc.ceo
resolves to GitHub Pages servers → HTTPS cert auto-provisioned by GitHub.
DNS changes propagate in 10 min – 48 hours (usually <1 hour).

## Pre-flight Checks
- [ ] Confirm deployment-001 handoff: `gh-pages` branch live, CNAME=arc.ceo
- [ ] Log into GoDaddy account → My Products → arc.ceo → Manage DNS
- [ ] Note current DNS records before making changes

## Context
Domain `arc.ceo` is registered on GoDaddy.
GitHub Pages requires specific DNS records to point a custom apex domain (arc.ceo)
and www subdomain to their servers.

## Tasks
**In GoDaddy DNS Management for arc.ceo:**

- [ ] **Delete** any existing A records for `@` (apex)
- [ ] **Add 4 A records** for `@` (apex) pointing to GitHub Pages IPs:
  ```
  Type: A   Host: @   Points to: 185.199.108.153   TTL: 600
  Type: A   Host: @   Points to: 185.199.109.153   TTL: 600
  Type: A   Host: @   Points to: 185.199.110.153   TTL: 600
  Type: A   Host: @   Points to: 185.199.111.153   TTL: 600
  ```
- [ ] **Add/update CNAME** for `www`:
  ```
  Type: CNAME   Host: www   Points to: <github-username>.github.io   TTL: 3600
  ```
  (replace `<github-username>` with your actual GitHub username)
- [ ] Save all DNS changes
- [ ] In GitHub repo Settings → Pages → Custom domain: type `arc.ceo` → Save
  (this verifies the CNAME file and initiates SSL provisioning)
- [ ] Wait for DNS propagation: `dig arc.ceo +noall +answer` should show the 4 IPs
- [ ] Wait for HTTPS cert: check GitHub repo Settings → Pages for "Your site is published"

## Acceptance Criteria
- [ ] `dig arc.ceo A` returns all 4 GitHub Pages IPs
- [ ] `https://arc.ceo` loads the ARC site (not GoDaddy placeholder)
- [ ] `https://www.arc.ceo` redirects to `https://arc.ceo`
- [ ] SSL cert valid (padlock in browser)
- [ ] No mixed content warnings

## Notes
- GitHub Pages IPs are stable but check https://docs.github.com/en/pages/configuring-a-custom-domain
  if this task is done months later
- TTL 600 (10 min) for A records means faster propagation during setup;
  increase to 3600 after confirmed working
- The `CNAME` file in the repo (created in deployment-001) is REQUIRED —
  without it, GitHub Pages won't recognize the custom domain after deploys
- If GoDaddy shows "Forwarding" is active on the domain, disable it first

---
**Session Handoff** (fill when done):
- Changed: GoDaddy DNS records for arc.ceo
- Causality: DNS A records → GitHub Pages → HTTPS cert auto-provisioned
- Verify: `curl -I https://arc.ceo` returns 200 OK
- Next: deployment-003 — production validation checklist