---
id: frontend-002
title: Add founder photo and section
status: done
priority: high
tags:
- frontend
dependencies:
- frontend-001
assignee: developer
created: 2026-03-04T09:00:24.892835872Z
estimate: 1h
complexity: 3
area: frontend
---

# Add founder photo and section

## Causation Chain
> Clean HTML (frontend-001) → add founder section → commit (setup-002) → deploy.
Photo asset must be in repo before GitHub Actions can serve it.

## Pre-flight Checks
- [ ] Confirm frontend-001 session handoff is complete
- [ ] Verify `assets/` directory exists (created in frontend-001)
- [ ] Source photo: `/data/eliran/Downloads/20260201_134610.jpg`

## Context
The site has no human presence — it's all philosophy and framework.
Adding a founder section with Eliran's photo creates trust and authenticity,
consistent with the email signature in the brand kit
(Eliran Sabag · Founder · ARC — Artificial Reasoning Corporation).

**Photo**: Desert canyon landscape photo (Eliran standing on rocks, wadi/canyon
background, natural light). Tone matches the brand — geological, expansive,
mathematically precise landscape.

**Placement**: New `#founder` section between `#mark-section` and `#contact`.

## Tasks
- [ ] Copy photo to `assets/eliran-sabag.jpg` (optimize if >500KB)
- [ ] Add `#founder` section to HTML between `#mark-section` and `#contact`
- [ ] Add `#founder` CSS: full-bleed image left, text right (or centered with overlay)
- [ ] Add nav link "Founder" pointing to `#founder`
- [ ] Add `.reveal` class to section elements for scroll animation
- [ ] Verify photo loads correctly and section is responsive

## Acceptance Criteria
- [ ] Photo visible at arc.ceo/#founder
- [ ] Name "Eliran Sabag" and title "Founder, ARC" displayed
- [ ] Section matches brand palette (dark background, gold accent, Outfit/JetBrains Mono)
- [ ] Responsive: photo and text stack on mobile

## Section Design
```
#founder layout:
  [photo — left half, full height, object-fit: cover]
  [right half: sec-label "Founder", name in h2 style, title in mono,
   1-2 sentence bio, thin gold rule]
```

Bio text suggestion:
> "Eliran Sabag is the founder of ARC — Artificial Reasoning Corporation.
> A decade of pattern recognition led to a single witnessed truth:
> the most important problems don't need more compute. They need the right mathematics."

## Notes
- Photo is ~3MB JPEG; compress to ≤300KB for web using `convert` or equivalent
- Use `object-position: center top` to keep Eliran's face in frame
- Keep `alt="Eliran Sabag, Founder of ARC"` for accessibility
- The section ID must be added to the mobile nav drawer (from frontend-001)

---
**Session Handoff** (fill when done):
- Changed: `index (1).html`, `assets/eliran-sabag.jpg`
- Causality: Photo served as static asset from `assets/`; section added to DOM
- Verify: open in browser, check photo loads, check mobile layout
- Next: setup-002 — all files ready to commit to git