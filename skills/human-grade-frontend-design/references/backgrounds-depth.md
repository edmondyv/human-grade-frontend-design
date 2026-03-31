# Backgrounds & Depth Reference

## Atmosphere Over Solid Colors

Create visual atmosphere through layered techniques rather than defaulting to flat solid colors.

## Layering Techniques

### Noise/Grain Overlay
```css
/* Inline SVG noise texture — no external file needed */
body::after {
  content: '';
  position: fixed;
  inset: 0;
  z-index: 9999;
  pointer-events: none;
  opacity: 0.03;
  background: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.8' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)'/%3E%3C/svg%3E");
}
```

### Gradient Mesh Background
```css
body {
  background:
    radial-gradient(ellipse at 20% 50%, rgba(120, 80, 40, 0.08) 0%, transparent 50%),
    radial-gradient(ellipse at 80% 20%, rgba(40, 80, 120, 0.06) 0%, transparent 50%),
    radial-gradient(ellipse at 50% 80%, rgba(80, 40, 80, 0.04) 0%, transparent 50%),
    var(--color-bg);
}
```

### Paper/Parchment Texture
```css
.paper-texture {
  background-color: var(--color-bg);
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='t'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.5' numOctaves='3' stitchTiles='stitch'/%3E%3CfeColorMatrix type='saturate' values='0'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23t)' opacity='0.04'/%3E%3C/svg%3E");
}
```

### Shadow Depth System
```css
:root {
  --shadow-sm: 0 1px 2px rgba(0,0,0,0.05);
  --shadow-md: 0 4px 12px rgba(0,0,0,0.08);
  --shadow-lg: 0 8px 24px rgba(0,0,0,0.12);
  --shadow-xl: 0 16px 48px rgba(0,0,0,0.16);
  --shadow-glow: 0 0 24px rgba(var(--accent-rgb), 0.2);
}
```

### Glass Morphism (Use Sparingly)
```css
.glass {
  background: rgba(255, 255, 255, 0.08);
  border: 1px solid rgba(255, 255, 255, 0.12);
  border-radius: 16px;
  /* Limit backdrop-filter to 3-5 elements max for performance */
  backdrop-filter: blur(16px);
  -webkit-backdrop-filter: blur(16px);
}
```

**Performance warning:** `backdrop-filter` is expensive. Limit to 3-5 elements per page (typically: nav, 1-2 hero elements, CTA). For other "glass" elements, use `background: rgba(...)` with higher opacity instead.

## Banned Visual Patterns

| Pattern | Why |
|---|---|
| "Fake dashboards" with generic red/yellow/green/blue callouts | Meaningless decoration pretending to be data |
| Non-system emojis as icons or decoration | Looks unfinished and unprofessional |
| Corporate Memphis / flat vector people illustrations | The most mocked AI/tech aesthetic |
| Iridescent gradients | AI cliche unless the product is literally about AI AND client requests it |
| Circuit board patterns | Same as above |
| Neural network imagery | Same as above |
| Stock photos of people pointing at screens | Generic and trust-destroying |

## Depth Creation Strategies

### Layered Sections
```css
.section--elevated {
  position: relative;
  z-index: 2;
  background: var(--color-surface);
  box-shadow: 0 -8px 32px rgba(0,0,0,0.08);
  border-radius: var(--radius-lg) var(--radius-lg) 0 0;
  margin-top: -24px; /* Overlap previous section */
}
```

### Floating Elements
```css
.floating-badge {
  position: absolute;
  top: -12px;
  right: -8px;
  background: var(--color-accent);
  padding: 4px 12px;
  border-radius: var(--radius-full);
  box-shadow: var(--shadow-md);
  transform: rotate(3deg);
}
```

### Background Transitions Between Sections
```css
.section--dark {
  background: var(--color-bg);
  color: var(--color-text);
}
.section--light {
  background: var(--color-surface);
  color: var(--color-text);
}
/* Alternate for visual rhythm */
```
