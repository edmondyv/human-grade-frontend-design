# Motion & Animation Reference

Every animation must serve a purpose: guide the eye, reinforce hierarchy, provide feedback, or create delight. Animation is NOT decoration.

## Page Load Orchestration

One well-orchestrated page load creates more perceived quality than 20 scattered micro-interactions:

```css
/* Staggered reveal pattern */
.hero-title { animation: fadeUp 600ms ease-out 100ms both; }
.hero-subtitle { animation: fadeUp 600ms ease-out 250ms both; }
.hero-cta { animation: fadeUp 600ms ease-out 400ms both; }
.hero-visual { animation: fadeUp 800ms ease-out 500ms both; }

@keyframes fadeUp {
  from { opacity: 0; transform: translateY(20px); }
  to { opacity: 1; transform: translateY(0); }
}
```

**Key rules:**
- Total orchestration window: 400-800ms (not longer)
- Stagger delay between elements: 100-200ms
- Each element's animation: 300-600ms
- Use `both` fill mode so elements stay visible

## Scroll-Triggered Reveals

Use IntersectionObserver with LOW thresholds and SHORT durations:

```javascript
const observer = new IntersectionObserver((entries) => {
  entries.forEach(entry => {
    if (entry.isIntersecting) {
      entry.target.classList.add('visible');
      observer.unobserve(entry.target);
    }
  });
}, { threshold: 0.1, rootMargin: '0px 0px -50px 0px' });
```

```css
.reveal {
  opacity: 0;
  transform: translateY(24px);
  transition: opacity 400ms ease-out, transform 400ms ease-out;
}
.reveal.visible {
  opacity: 1;
  transform: translateY(0);
}
```

**Critical rules:**
- Threshold: 0.1-0.2 (trigger early, not when element is centered)
- Duration: 300-500ms max (users scroll past longer animations)
- Unobserve after triggering (performance + prevents re-animation)
- Content must be visible by the time the user reaches it — never after

## Hover Feedback

Subtle, consistent hover states on interactive elements:

```css
.card {
  transition: transform 200ms ease-out, box-shadow 200ms ease-out;
}
.card:hover {
  transform: translateY(-2px) scale(1.02);
  box-shadow: 0 8px 24px rgba(0,0,0,0.12);
}

.button {
  transition: background 150ms ease, transform 150ms ease;
}
.button:hover {
  transform: scale(1.03);
}
.button:active {
  transform: scale(0.98);
}
```

**Scale ranges:** 1.02-1.05 for cards, 1.03-1.05 for buttons. Never more than 1.08.

## Scroll-Driven Canvas Animations

For complex scroll-linked animations (particles, SVG paths, physics):

```javascript
// Shared requestAnimationFrame loop — never multiple rAF loops
let scrollY = 0;
let ticking = false;

window.addEventListener('scroll', () => { scrollY = window.scrollY; }, { passive: true });

function animate() {
  // All scroll-driven updates in one loop
  updateParticles(scrollY);
  updateSVGPath(scrollY);
  requestAnimationFrame(animate);
}
requestAnimationFrame(animate);
```

**Performance rules:**
- ONE shared rAF loop for all animations
- Passive scroll listener (never preventDefault)
- Read scroll position from variable, not DOM (avoids forced reflow)
- Use `transform` and `opacity` only (composited, GPU-accelerated)
- Avoid `will-change` on more than 5-10 elements
- Use `contain: layout style paint` on animated containers

## Reduced Motion Support

Always implement both CSS and JavaScript checks:

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
  }
}
```

```javascript
const prefersReducedMotion = window.matchMedia('(prefers-reduced-motion: reduce)').matches;
if (prefersReducedMotion) {
  // Skip canvas animations, show static content
  document.querySelectorAll('.reveal').forEach(el => el.classList.add('visible'));
  return;
}
```

## Banned Animation Behaviors

**Never do these:**

| Behavior | Why It's Bad |
|---|---|
| Scroll-jacking | Disorients users, feels like wading through molasses |
| Continuous cursor-followers | Draws eye away from content |
| Shooting lines / orbiting dots | Meaningless decoration |
| Pulsing elements | Anxiety-inducing, distracting |
| Floating particles with no purpose | Classic AI slop |
| Fade-in > 400ms on scroll | Users scroll past before animation completes |
| Parallax on every element | Nauseating, performance-killing |
| Transform on hover > scale(1.08) | Looks broken, not polished |

## Easing Functions

```css
:root {
  --ease-out-expo: cubic-bezier(0.16, 1, 0.3, 1);
  --ease-out-quart: cubic-bezier(0.25, 1, 0.5, 1);
  --ease-in-out-quart: cubic-bezier(0.76, 0, 0.24, 1);
  --spring: cubic-bezier(0.34, 1.56, 0.64, 1);
}
```

- **ease-out-expo**: Best for elements entering the viewport (fast start, gentle settle)
- **ease-out-quart**: Good default for most transitions
- **spring**: For playful, bouncy interactions (use sparingly)
- **linear**: Only for continuous rotation or progress bars

---

## Performance: The "Death Trio" and Other Killers

Visual effects become performance killers when they force the browser to recalculate layout (Reflow) or redraw pixels (Repaint) on every frame.

### 1. Blurs, Shadows, and Scrolling — The Death Trio

The most dangerous combination: `backdrop-filter: blur()` inside a `position: sticky` or `position: fixed` element (like a nav bar).

**Why it kills performance:** Every scroll frame, the browser re-calculates the blur for every pixel behind that element in real-time.

**Avoid:** Combining `backdrop-filter: blur()` with high-frequency scroll events or placing it over moving/video content.

**Fix:** Use a semi-transparent solid color (`background: rgba(0,0,0,0.85)`) or a pre-blurred static background image. Reserve `backdrop-filter` for 3-5 elements max per page (nav, hero glass panel, CTA).

### 2. Layout-Triggering Animations

Never animate properties that change page geometry — these force full layout reflow:

| Avoid Animating | Use Instead |
|---|---|
| `width`, `height` | `transform: scale()` |
| `top`, `left` | `transform: translate()` |
| `margin`, `padding` | `transform: translate()` |
| `font-size` | `transform: scale()` |
| `flex-basis` transitions on nested elements | Fixed dimensions + `transform` |

The worst combo: Animating `height: auto` or `flex-basis` on multiple nested elements causes "layout thrashing" — the browser gets stuck in a recalculation loop.

### 3. Over-Stacking Expensive CSS

Some properties require complex per-pixel math:

**Avoid combining on many elements at once:**
- Large `box-shadow` (high blur radius) + `opacity` + `border-radius`
- Soft-edged shadows are Gaussian blurs — computationally heavy to paint
- Applying these to hundreds of cards in a grid makes scrolling choppy on mobile

**Fix:** Use `filter: drop-shadow()` sparingly, or bake shadows into background images for complex decorative effects.

### 4. JavaScript Scroll Listeners vs. Native CSS

JS-based scroll listeners block the Main Thread. The worst combo: JS scroll listeners + `filter: blur()` or `filter: grayscale()`.

**Modern alternatives (in order of preference):**

1. **CSS Scroll-Driven Animations** (best performance — runs on compositor thread):
```css
@supports (animation-timeline: view()) {
  .reveal {
    animation: fadeIn linear both;
    animation-timeline: view();
    animation-range: entry 0% entry 100%;
  }
}
```

2. **IntersectionObserver** (good — fires only on threshold crossings):
```javascript
const observer = new IntersectionObserver(callback, { threshold: 0.1 });
```

3. **GSAP ScrollTrigger with Lenis** (good for complex sequences):
```javascript
// Sync Lenis with GSAP ticker
lenis.on('scroll', ScrollTrigger.update);
gsap.ticker.add((time) => lenis.raf(time * 1000));
gsap.ticker.lagSmoothing(0);
```

4. **Raw scroll event listener** (last resort — always use `{ passive: true }`):
```javascript
window.addEventListener('scroll', handler, { passive: true });
```

### Property Performance Reference

| Performance | Properties (Safe to Animate) | Properties (Avoid) |
|---|---|---|
| **High (GPU/Compositor)** | `transform`, `opacity` | — |
| **Medium (Paint only)** | `color`, `background-color` | `box-shadow`, `filter`, `backdrop-filter` |
| **Low (Triggers Layout)** | — | `width`, `height`, `margin`, `top`, `left`, `padding`, `font-size` |

### The `contain` Property

Use `contain: layout style paint` on animated containers to tell the browser the element won't affect layout outside its bounds:

```css
.animated-card {
  contain: layout style paint;
}
```

This reduces compositor work significantly when many elements are animated.

### `will-change` Budget

Only apply `will-change` to elements with continuous CSS animations (e.g., rotating backgrounds, moving particles). Remove it from elements that only animate once (scroll reveals). Budget: max 5-10 elements per page.

---

## Advanced Scroll Animation Patterns

Techniques used by Awwwards and Godly-featured sites:

### Sticky Grid Scroll
Pin a container with `position: sticky` and use a tall parent (e.g., `height: 425vh`) to create temporal space for phased animations. Map scroll progress to distinct phases (0-45% reveal, 45-90% zoom, 90-100% settle).

### Split Text Animation
Split headlines into individual characters/words using SplitType or GSAP SplitText. Animate each element independently with staggered delays:
```javascript
// Stagger characters from center outward
gsap.from(chars, {
  y: 40, opacity: 0, duration: 0.6,
  stagger: { each: 0.03, from: 'center' }
});
```

### Horizontal Scroll Sections
Map vertical scroll to horizontal movement using sticky positioning + translateX:
```css
.horizontal-track {
  position: sticky;
  top: 0;
  display: flex;
  /* translateX driven by scroll via JS or scroll-timeline */
}
```

### Reveal/Mask Animations
Use `clip-path` transitions to reveal content with organic shapes:
```css
.reveal { clip-path: circle(0% at 50% 50%); }
.reveal.visible { clip-path: circle(100% at 50% 50%); transition: clip-path 800ms var(--ease-out-expo); }
```

### Magnetic/Cursor-Following Elements
Elements that subtly follow cursor position (for hero CTAs or feature highlights). Use lerp (linear interpolation) for smooth following:
```javascript
// Lerp toward cursor position
x += (targetX - x) * 0.08;
y += (targetY - y) * 0.08;
element.style.transform = `translate(${x}px, ${y}px)`;
```
**Warning:** Only use on 1-2 hero elements. Never as a persistent cursor follower.
