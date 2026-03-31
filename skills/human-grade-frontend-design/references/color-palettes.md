# Color & Palette Reference

## Strategy

- Define a complete palette using CSS custom properties (variables). Non-negotiable.
- Use a dominant color with ONE sharp accent. Timid, evenly-distributed palettes look AI-generated.
- Pull inspiration from unexpected sources: film color grading, Japanese design, brutalist architecture, vintage posters, IDE themes, fashion editorials.
- Vary between light and dark themes across projects. Do NOT always default to dark.

## Banned Palettes

These are the most overused AI-generated color combinations. Never use them:

| Banned Combination | Why |
|---|---|
| Purple/violet gradients on white backgrounds | The #1 AI cliche — every AI demo site uses this |
| Purple + teal/cyan combinations | Second most common AI default |
| Generic blue SaaS (#4F46E5 → #06B6D4 gradients) | Tailwind default → every AI picks it |
| Rainbow gradients pretending to be "modern" | Screams "I couldn't commit to a direction" |

## Proven Palettes

These work. Adapt them — don't copy verbatim:

### Dark Themes
```css
/* Warm charcoal + terracotta */
:root {
  --bg: #1a1a1a;
  --surface: #242424;
  --accent: #c84b31;
  --text: #f5f0eb;
  --text-dim: #a89f95;
}

/* Deep navy + gold */
:root {
  --bg: #0a1628;
  --surface: #0f1d35;
  --accent: #d4a853;
  --text: #e8e4dd;
  --text-dim: #64748b;
}

/* Near-black + electric coral */
:root {
  --bg: #0d0d0d;
  --surface: #1a1a1a;
  --accent: #ff6b5a;
  --text: #f0ede8;
  --text-dim: #6b6b6b;
}

/* Near-black + lime */
:root {
  --bg: #0d0d0d;
  --surface: #1a1a1a;
  --accent: #BFFF00;
  --text: #f0ede8;
  --text-dim: #6b6b6b;
}
```

### Light Themes
```css
/* Cream + forest green */
:root {
  --bg: #fafaf5;
  --surface: #f0ede6;
  --accent: #2d5016;
  --accent-secondary: #b87333;
  --text: #1a1a1a;
  --text-dim: #5c5c5c;
}

/* Warm cream + terracotta */
:root {
  --bg: #f5f0eb;
  --surface: #ebe5dd;
  --accent: #c84b31;
  --text: #1a1a1a;
  --text-dim: #6b5d52;
}

/* Paper white + ink */
:root {
  --bg: #f8f5ef;
  --surface: #f0ede6;
  --accent: #1a1510;
  --accent-secondary: #c44536;
  --text: #1a1a1a;
  --text-dim: #6b6b6b;
}
```

## CSS Custom Properties Pattern

Always structure palettes with semantic naming:

```css
:root {
  /* Core */
  --color-bg: #...;
  --color-surface: #...;
  --color-surface-elevated: #...;

  /* Text */
  --color-text: #...;
  --color-text-secondary: #...;
  --color-text-muted: #...;

  /* Accent */
  --color-accent: #...;
  --color-accent-hover: #...;
  --color-accent-dim: #...;

  /* Borders & Shadows */
  --color-border: #...;
  --color-shadow: rgba(...);
}
```

## Inspiration Sources

When choosing palettes, draw from:
- **Film**: Wes Anderson pastels, Blade Runner 2049 amber/teal, Dune desert tones
- **Japanese design**: Wabi-sabi earth tones, ukiyo-e printing inks
- **Architecture**: Brutalist concrete + single accent, Scandinavian birch + green
- **Fashion**: Bottega Veneta green, Hermès orange, Acne Studios pink
- **Nature**: Mediterranean terracotta, Nordic forest, desert sunset
- **IDE themes**: Dracula, One Dark, Solarized (as starting points, not endpoints)

## Anti-Patterns

- Using more than 2 accent colors (1 is ideal, 2 max)
- Hardcoding hex values instead of CSS variables
- Gradients between complementary colors (looks muddy)
- Using opacity for everything instead of defining intentional color steps
- Dark mode that's just "invert the colors" (needs its own palette)
