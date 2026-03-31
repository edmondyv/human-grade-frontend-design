# Layout & Spatial Composition Reference

## Core Principles

- Break away from predictable layouts — use asymmetry, overlap, diagonal flow, and grid-breaking hero elements where they serve the design.
- One clear focal point per viewport. Never let elements compete for attention.
- Responsive from the start: design mobile-first, then expand.
- Containers should breathe. Max-width the content (1200-1400px), let backgrounds bleed full-width.

## Spacing Scale

Vary spacing rhythmically — identical padding/margins everywhere is an AI tell:

```css
:root {
  --space-xs: 4px;
  --space-sm: 8px;
  --space-md: 16px;
  --space-lg: 24px;
  --space-xl: 32px;
  --space-2xl: 48px;
  --space-3xl: 64px;
  --space-4xl: 96px;

  /* Section gaps — responsive */
  --section-gap: clamp(60px, 10vw, 120px);
  --section-gap-sm: clamp(40px, 6vw, 80px);
}
```

**Apply intentionally:** A hero might use `--space-4xl` bottom padding, while a feature section uses `--space-3xl`. This rhythmic variation creates visual breathing.

## Grid-Breaking Techniques

### Asymmetric Hero
```css
.hero {
  display: grid;
  grid-template-columns: 1.2fr 0.8fr;
  gap: var(--space-3xl);
  align-items: center;
}
```

### Overlapping Elements
```css
.feature-visual {
  position: relative;
  z-index: 1;
}
.feature-card {
  position: relative;
  z-index: 2;
  margin-top: -48px; /* Intentional overlap */
  margin-left: 32px;
}
```

### Staggered Grid
```css
.grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: var(--space-xl);
}
.grid > :nth-child(2) {
  transform: translateY(32px); /* Offset for visual interest */
}
```

### Full-Bleed with Contained Content
```css
.section {
  width: 100%;
  padding: var(--section-gap) var(--space-lg);
}
.section-inner {
  max-width: var(--max-width);
  margin: 0 auto;
}
```

## Banned Layout Patterns

| Pattern | Why It's Banned |
|---|---|
| Cookie-cutter bento box grids | No context-specific character — every SaaS AI output looks identical |
| Three-column feature grids (icon + title + paragraph) | The single most overused SaaS pattern |
| Identical sections repeating same structure | Copy-paste layout with swapped content = instant AI tell |
| Centered everything | Predictable, lacks visual tension |
| Full-width cards with no max-width | Unreadable on large screens |

## Alternative Layout Strategies

Instead of 3-column icon grids, try:

- **Alternating left/right blocks** with different aspect ratios
- **Single spotlight feature** with supporting details below
- **Horizontal scroll carousel** for feature cards (with overflow hints)
- **Stacked full-width sections** with distinct visual treatments per section
- **Masonry or asymmetric grid** with varying card sizes
- **Timeline/path layout** connecting features in a narrative flow

## Responsive Breakpoints

```css
/* Mobile first — base styles are mobile */

/* Tablet landscape */
@media (min-width: 768px) { ... }

/* Desktop */
@media (min-width: 1024px) { ... }

/* Large desktop */
@media (min-width: 1440px) { ... }
```

**Common responsive patterns:**
- 1-column on mobile → 2-column on tablet → 3-column on desktop
- Stack hero side-by-side on desktop, vertically on mobile
- Reduce section padding by ~40% on mobile
- Hide decorative elements on mobile (keep focus on content)

## Container Pattern

```css
.container {
  width: 100%;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 clamp(16px, 4vw, 32px);
}

.container--narrow { max-width: 800px; }
.container--wide { max-width: 1400px; }
```

## Hover & Interaction States

- Every interactive element must have visible hover AND focus states.
- `cursor: pointer` on all clickable elements.
- Never hide navigation, CTAs, pricing, or critical info behind hover (mobile has no hover).
- Remove `-webkit-tap-highlight-color` blue flash on mobile.
- Add `touch-action: manipulation` on tap targets.

```css
/* Remove mobile tap highlight */
* { -webkit-tap-highlight-color: transparent; }

/* Visible focus states */
:focus-visible {
  outline: 2px solid var(--color-accent);
  outline-offset: 2px;
}
```
