---
name: human-grade-frontend-design
description: >
  This skill should be used when the user asks to "build a landing page", "create a website",
  "design a frontend", "make a web app", "build a dashboard", "create a SaaS page",
  "design a hero section", "build a signup page", "create a portfolio site",
  "make a marketing page", "design a pricing page", "build a checkout flow",
  or any request involving HTML/CSS/JS interface creation, web design, UI development,
  frontend prototyping, or visual web development. Also triggers on "make it look good",
  "improve the design", "fix the styling", "make it less generic", "remove AI slop",
  or complaints about generic/cookie-cutter/boring designs.
version: 1.0.0
---

# Frontend Design System

A comprehensive, opinionated design system for creating production-grade web interfaces that feel unmistakably human-crafted. Informed by 57 peer-reviewed papers on prompt engineering.

**Core principle:** Actively fight convergence toward safe, predictable, generic "AI slop" designs on every decision — font choice, color palette, layout structure, animation approach.

---

## Design Thinking (Before Any Code)

Stop. Before writing a single line, answer these four questions:

1. **Purpose & User**: What does this interface accomplish? Who is the target user? What's their emotional state when they arrive? What action should they take?

2. **Aesthetic Direction**: Commit to ONE bold direction and execute with conviction. Not "modern and clean" — that's nothing. Choose from real directions:
   - Brutally minimal (Dieter Rams meets web)
   - Luxury editorial (Vogue meets SaaS)
   - Neo-brutalist (raw, unapologetic, high contrast)
   - Warm organic (earth tones, natural textures, rounded forms)
   - Swiss precision (grid worship, mathematical spacing)
   - Retro-futuristic (CRT glow, scanlines, monospace)
   - Soft maximalism (layered, textured, rich but controlled)

3. **The Memory Test**: What is the ONE visual element someone remembers 24 hours later? Anchor the design around that.

4. **Anti-Default Check**: For every choice, ask "is this what an AI would pick by default?" If yes, choose differently.

---

## Quick Reference: Critical Rules

### Typography
- Pair a distinctive display font with a refined body font. Both must have character.
- ONE hero treatment (H1 + subtext), not 4-5 competing text styles per block.
- Load fonts via Google Fonts, Adobe Fonts, or self-hosted WOFF2.
- **See `references/typography.md`** for complete banned font list, convergence traps, and recommended pairings.

### Color & Theme
- Define ALL colors as CSS custom properties. Non-negotiable.
- Use a dominant color with ONE sharp accent. Timid palettes look AI-generated.
- Vary between light and dark themes across projects.
- **See `references/color-palettes.md`** for banned palettes, example palettes, and inspiration sources.

### Hero Section
- H1 must instantly communicate: **what the product is**, **who it's for**, **why they should care**.
- Clear, prominent CTA (button, not link) above the fold.
- Do NOT use full 100vh background. Leave ~10px of next section peeking as a scroll hint.
- Subtext: one sentence max.

### Motion & Animation
- Every animation must serve a purpose: guide the eye, reinforce hierarchy, provide feedback, or create delight.
- `prefers-reduced-motion` support — always. Both CSS media query AND JavaScript check.
- **See `references/animation-guide.md`** for scroll patterns, performance rules, and banned behaviors.

### Layout
- Break away from predictable layouts — asymmetry, overlap, diagonal flow where they serve the design.
- One clear focal point per viewport. Never let elements compete for attention.
- Responsive from the start: test at 320px, 768px, 1024px, 1440px.
- **See `references/layout-patterns.md`** for spatial composition rules and banned patterns.

### Code Quality
- Semantic HTML: `<header>`, `<nav>`, `<main>`, `<section>`, `<footer>`. Not div soup.
- CSS custom properties for all colors, fonts, spacing, transitions.
- All interactive elements keyboard-navigable (tab order, focus states).
- For prototypes: single-file HTML with all CSS/JS inline.

---

## Workflow

### Step 1: Design Decisions
Answer the four design thinking questions above. Pick an aesthetic direction.

### Step 2: Select Typography
Consult `references/typography.md` for font pairing. Choose a display + body + optional mono font. Verify none are on the banned list.

### Step 3: Define Color Palette
Consult `references/color-palettes.md`. Define 4-6 colors as CSS custom properties. Verify against banned palettes.

### Step 4: Build
Write production-grade code following the rules above. For each section, reference `references/layout-patterns.md` for spatial composition and `references/animation-guide.md` for motion.

### Step 5: Self-Verify
Run the complete checklist in `references/verification-checklist.md` before delivering. Every "AI Slop Detection" item must be FALSE. Every "Quality Standard" item must be TRUE. Fix any failures before showing the output.

---

## Conversion & Trust

Design serves the business goal:
- Visual hierarchy guides the eye: Headline → Value Prop → Social Proof → CTA.
- Primary CTA: unmissable, above fold, high contrast. Use action verbs ("Start free trial", not "Learn more").
- Trust signals: specific numbers beat vague claims ("$2M saved" > "Trusted by companies worldwide").
- Forms: minimize fields. Email-only > 5-field form.

---

## Backgrounds & Depth

Create atmosphere, not solid colors:
- Layer techniques: gradient meshes, noise/grain overlays (CSS SVG), geometric patterns, subtle texture.
- Dramatic shadows matching the chosen aesthetic.
- **See `references/backgrounds-depth.md`** for techniques and banned patterns.

---

## Additional Resources

### Reference Files
- **`references/typography.md`** — Complete font system: banned fonts, convergence traps, recommended pairings, loading strategies
- **`references/color-palettes.md`** — Palette system: banned combos, proven palettes, CSS variable patterns, inspiration sources
- **`references/animation-guide.md`** — Motion system: scroll-triggered patterns, page load orchestration, hover feedback, performance rules, banned behaviors
- **`references/layout-patterns.md`** — Spatial composition: grid-breaking techniques, spacing scales, responsive strategies, banned layout patterns
- **`references/backgrounds-depth.md`** — Depth system: layering techniques, texture patterns, shadow strategies, banned visual patterns
- **`references/verification-checklist.md`** — Complete self-audit: AI slop detection (must all be FALSE) + quality standards (must all be TRUE)

### Examples
- **`examples/saas-landing.md`** — Complete SaaS landing page design walkthrough with decision rationale
- **`examples/dashboard-app.md`** — Dashboard/web app design walkthrough with component patterns

---

## The Standard

Think of the best Framer templates, Awwwards winners, and Webflow Showcases. That's the floor, not the ceiling. Every pixel must feel like it was placed by a senior designer who spent hours on the details.
