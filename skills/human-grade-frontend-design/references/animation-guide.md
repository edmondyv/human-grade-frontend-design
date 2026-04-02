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

---

## Component Library Animation Patterns

Modern animated component libraries have established new standards for interaction design. These patterns can be adapted to vanilla CSS/JS:

### Spotlight / Glow Card Effect (Aceternity UI)
A radial gradient follows the cursor over a card, creating a spotlight glow:
```css
.card::before {
  content: '';
  position: absolute;
  top: var(--mouse-y, 50%);
  left: var(--mouse-x, 50%);
  width: 400px;
  height: 400px;
  background: radial-gradient(circle, var(--accent-muted) 0%, transparent 70%);
  transform: translate(-50%, -50%);
  opacity: 0;
  transition: opacity 0.4s ease;
  pointer-events: none;
}
.card:hover::before { opacity: 1; }
```
JS sets `--mouse-x` and `--mouse-y` on mousemove. Best for: feature cards, pricing cards.

### Shimmer Border (Magic UI)
An animated conic gradient rotates around a card border using `@property`:
```css
@property --shimmer-angle {
  syntax: '<angle>';
  initial-value: 0deg;
  inherits: false;
}
.featured-card::before {
  content: '';
  position: absolute;
  inset: -2px;
  border-radius: inherit;
  background: conic-gradient(from var(--shimmer-angle), var(--accent), var(--secondary), var(--accent));
  z-index: -1;
  animation: shimmer-rotate 4s linear infinite;
}
@keyframes shimmer-rotate { to { --shimmer-angle: 360deg; } }
```
Best for: featured/highlighted cards, premium CTAs, pricing tiers.

### Gradient Text Shimmer (Magic UI)
Text with an animated gradient that sweeps across:
```css
.shimmer-text {
  background: linear-gradient(90deg, var(--accent), var(--secondary), var(--accent));
  background-size: 200% 100%;
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  animation: text-shimmer 4s ease-in-out infinite;
}
@keyframes text-shimmer {
  0%, 100% { background-position: 0% 50%; }
  50% { background-position: 100% 50%; }
}
```
Best for: hero accent text, feature titles, premium labels.

### Magnetic Button (React Bits)
Button shifts toward cursor on hover using mousemove to calculate offset:
```javascript
btn.addEventListener('mousemove', function(e) {
  var rect = btn.getBoundingClientRect();
  var x = (e.clientX - rect.left - rect.width / 2) * 0.15;
  var y = (e.clientY - rect.top - rect.height / 2) * 0.3;
  btn.style.transform = 'translate(' + x + 'px, ' + y + 'px)';
});
btn.addEventListener('mouseleave', function() {
  btn.style.transform = 'translate(0, 0)';
});
```
Best for: hero CTAs, single primary actions. **Never** on navigation or form buttons.

### Animated Number Ticker
Counts up from 0 with ease-out deceleration, triggered by IntersectionObserver:
```javascript
function animateCounter(el, target, duration) {
  var startTime = null;
  function ease(t) { return 1 - Math.pow(1 - t, 4); }
  function step(ts) {
    if (!startTime) startTime = ts;
    var p = Math.min((ts - startTime) / duration, 1);
    el.textContent = Math.floor(target * ease(p)).toLocaleString();
    if (p < 1) requestAnimationFrame(step);
  }
  requestAnimationFrame(step);
}
```
Best for: social proof stats, metrics sections, pricing numbers.

### 3D Tilt Card (Aceternity UI / React Bits)
Card tilts in 3D space following the cursor, with inner elements shifting at different rates for parallax depth:
```css
.card-container { perspective: 1000px; }
.card {
  transform-style: preserve-3d;
  transition: transform 0.15s ease-out;
}
.card .inner-elevated { transform: translateZ(30px); }
```
```javascript
card.addEventListener('mousemove', function(e) {
  var rect = card.getBoundingClientRect();
  var rotateY = ((e.clientX - (rect.left + rect.width / 2)) / (rect.width / 2)) * 8;
  var rotateX = -((e.clientY - (rect.top + rect.height / 2)) / (rect.height / 2)) * 8;
  card.style.transform = 'rotateX(' + rotateX + 'deg) rotateY(' + rotateY + 'deg)';
});
card.addEventListener('mouseleave', function() {
  card.style.transform = 'rotateX(0) rotateY(0)';
});
```
Best for: pricing cards, feature showcases, product cards. Keep rotation subtle (6-10 deg max).

### Aurora / Mesh Gradient Background (Aceternity UI)
Large blurred blobs drift slowly across a section, blending like northern lights:
```css
.aurora-blob {
  position: absolute;
  width: 40vmax;
  height: 40vmax;
  border-radius: 50%;
  filter: blur(80px);
  opacity: 0.1;
  will-change: transform;
}
.blob-1 {
  background: var(--accent);
  animation: drift1 18s ease-in-out infinite;
}
.blob-2 {
  background: var(--secondary);
  animation: drift2 22s ease-in-out infinite;
}
@keyframes drift1 {
  0%, 100% { transform: translate(-10%, -5%); }
  50% { transform: translate(15%, 10%); }
}
```
Best for: hero sections, CTA sections, above-the-fold drama. Keep opacity very low (0.06-0.12).

### Split-Letter Text Animation (React Bits BlurText)
Each letter animates in individually with staggered delay and blur-to-clear transition:
```javascript
heading.innerHTML = heading.textContent.split('').map(function(char, i) {
  return '<span style="--i:' + i + '" class="letter">' +
    (char === ' ' ? '&nbsp;' : char) + '</span>';
}).join('');
```
```css
.letter {
  display: inline-block;
  opacity: 0;
  filter: blur(8px);
  transform: translateY(16px);
  animation: letterReveal 0.4s ease forwards;
  animation-delay: calc(var(--i) * 0.03s);
}
@keyframes letterReveal {
  to { opacity: 1; filter: blur(0); transform: translateY(0); }
}
```
Best for: hero headlines, section titles, dramatic text entrances. Trigger via IntersectionObserver.

### Scroll-Driven Clip-Path Reveal (CSS-only, Chromium)
Elements reveal progressively as you scroll, with no JS needed:
```css
@keyframes clipReveal {
  from { clip-path: inset(0 100% 0 0); opacity: 0; }
  to { clip-path: inset(0 0 0 0); opacity: 1; }
}
.scroll-reveal {
  animation: clipReveal linear both;
  animation-timeline: view();
  animation-range: entry 10% cover 40%;
}
```
**Note:** `animation-timeline: view()` is Chromium-only (Chrome 115+). Always pair with an IntersectionObserver fallback for Firefox/Safari. Best for: feature sections, testimonials, content blocks.

### Key Libraries Reference
| Library | URL | Best For | Can Adapt to Vanilla |
|---|---|---|---|
| **Aceternity UI** | aceternity.com | Card effects, text animations, backgrounds | Yes — CSS + minimal JS |
| **Magic UI** | magicui.design | Borders, gradients, number tickers | Yes — CSS @property + JS |
| **21st.dev** | 21st.dev | Component discovery, design patterns | Conceptual inspiration |
| **React Bits** | reactbits.dev | Cursor effects, text animations, magnetics | Yes — mousemove JS |
| **Framer Motion** | framer.com/motion | Layout animations, spring physics | Partial — CSS springs |
| **GSAP** | gsap.com | Timeline sequences, ScrollTrigger | Yes — direct use |
