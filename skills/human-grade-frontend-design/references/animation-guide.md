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
