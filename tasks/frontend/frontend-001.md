---
id: frontend-001
title: 'Fix HTML issues: SVG logo, mobile nav, duplicate tagline'
status: done
priority: high
tags:
- frontend
dependencies:
- setup-001
assignee: developer
created: 2026-03-04T09:00:21.917569717Z
estimate: 1h
complexity: 4
area: frontend
---

# Fix HTML issues: SVG logo, mobile nav, duplicate tagline

## Causation Chain
> HTML quality must be clean before adding new sections (frontend-002),
before committing to git (setup-002), before deploying (deployment-001).
Broken markup → broken deploy → broken live site.

## Pre-flight Checks
- [ ] Read `index (1).html` — the single-file static site
- [ ] Review ARC BRANDING.pdf for correct SVG mark geometry
- [ ] Confirm all three issues are present before editing

## Context
Three bugs found during brand review of `index (1).html`:

1. **SVG logo is a rough approximation** — the inline SVG in nav and hero-bg
   uses simple strokes (`M30 6 L8 54`, `M30 6 L52 54`, quadratic bezier arc).
   The brand mark (per PDF) is a filled A-shape with thick legs and an arc
   cutting through. A proper `arc-mark-white.svg` asset file should exist and
   be referenced, OR the inline SVG should be replaced with a filled-path
   version matching the brand mark geometry.

2. **Duplicate tagline** — both `.hero-tag` (line ~397) and `.hero-tagline`
   (line ~399) display the same text. `.hero-tag` shows
   "Artificial Reasoning Corporation · 2026" AND "Bounded Search, Not Brute
   Force" is already in `.hero-tagline`. The `.hero-tag` should only show the
   corp name + year, not the tagline again.

3. **Mobile nav has no replacement** — `.nav-links { display:none; }` at
   ≤880px with no hamburger/drawer. Users on mobile see no navigation at all.

## Tasks
- [ ] Create `assets/` directory and `arc-mark-white.svg` with correct filled-path geometry
- [ ] Update `<img>` or `<svg>` references in nav and hero to use the asset file
- [ ] Fix `.hero-tag` text to only show "Artificial Reasoning Corporation · 2026"
- [ ] Add hamburger button + mobile drawer nav (CSS-only toggle via checkbox or minimal JS)
- [ ] Open in browser to visually verify all three fixes

## Acceptance Criteria
- [ ] Logo matches brand mark shape (filled A with arc cut-through)
- [ ] Hero area shows tagline exactly once
- [ ] On viewport ≤880px a menu icon appears and tapping it reveals nav links

## Notes
- The SVG mark geometry from the brand PDF: two thick descending legs from apex,
  a curved arc (like a bowl) cutting across the lower portion of the legs.
  Approximate filled paths: left leg `M200,60 L80,380 L130,380 L200,160 Z`,
  right leg `M200,60 L320,380 L270,380 L200,160 Z`,
  arc as a thick filled crescent shape across the base.
- Keep the CSS custom properties (--black, --gold, etc.) — they are correct.
- Mobile nav should match the existing style: JetBrains Mono, uppercase, muted color.

---
**Session Handoff** (fill when done):
- Changed: `index (1).html`, new `assets/arc-mark-white.svg`
- Causality: SVG referenced by nav and hero; tagline in hero-tag; nav toggle JS
- Verify: open in browser, resize to mobile
- Next: frontend-002 can now safely add a new section without merging into broken markup