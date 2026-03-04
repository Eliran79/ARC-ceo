---
id: deployment-003
title: Production validation and SSL verification
status: doing
priority: high
tags:
- deployment
dependencies:
- deployment-002
assignee: developer
created: 2026-03-04T09:06:02.020550446Z
estimate: 30m
complexity: 2
area: deployment
---

# Production validation and SSL verification

## Causation Chain
> DNS live (deployment-002) → validate everything works end-to-end before
announcing the site publicly. This is the final gate.

## Pre-flight Checks
- [ ] Confirm deployment-002 handoff: arc.ceo DNS updated
- [ ] Wait minimum 10 minutes after DNS change before starting validation
- [ ] Have both desktop browser and mobile device ready

## Context
Final validation that arc.ceo is correctly live with HTTPS, all assets load,
and the full user experience works as intended before any public announcement.

## Tasks
**Functional checks:**
- [ ] `curl -I https://arc.ceo` → HTTP 200 OK
- [ ] `curl -I http://arc.ceo` → HTTP 301 redirect to https://
- [ ] `curl -I https://www.arc.ceo` → redirect to https://arc.ceo
- [ ] Open https://arc.ceo in browser → site loads fully

**Visual checks:**
- [ ] ARC logo renders correctly in nav and hero
- [ ] All 6 pillars display
- [ ] Founder photo loads (assets/eliran-sabag.jpg)
- [ ] Ticker animation runs
- [ ] Scroll reveal animations trigger
- [ ] Contact link `contact@arc.ceo` is clickable

**Mobile checks (≤880px):**
- [ ] Hamburger/mobile nav appears and works
- [ ] Founder section stacks correctly
- [ ] No horizontal overflow

**Performance:**
- [ ] Page load <3s on standard connection
- [ ] No console errors in browser devtools
- [ ] Google Fonts load (Outfit, JetBrains Mono)

**SEO / meta:**
- [ ] `<title>` = "ARC — Artificial Reasoning Corporation"
- [ ] Meta description present
- [ ] Favicon shows in browser tab

## Acceptance Criteria
- [ ] `https://arc.ceo` returns 200 with valid SSL cert
- [ ] All assets (SVG, photo, fonts) load without 404 errors
- [ ] Site is usable on mobile
- [ ] No console errors

## Notes
- SSL cert provisioning can take up to 24h after DNS change; the `*.github.io`
  URL will work immediately if you need to test before DNS propagates
- Use `https://www.ssllabs.com/ssltest/analyze.html?d=arc.ceo` for SSL grade
- Use Google PageSpeed Insights for performance score
- Once validated, share the URL publicly

---
**Session Handoff** (fill when done):
- Changed: nothing (validation only)
- Causality: arc.ceo → GitHub Pages → HTTPS cert → live site
- Verify: all checkboxes above
- Next: **DONE** — arc.ceo is live. Update MEMORY.md with live URL.