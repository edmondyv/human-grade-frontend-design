# Typography Reference

Typography is the #1 differentiator between AI-generated and human-designed interfaces.

## Requirements

- Pair a distinctive display font with a refined body font. Both must have character.
- Keep hierarchy tight: ONE hero treatment (H1 + subtext), not 4-5 competing text styles per block.
- Ensure high contrast and legibility — especially navigation text over dynamic/video backgrounds.
- Load fonts via Google Fonts, Adobe Fonts, or self-hosted WOFF2.

## Banned Fonts (Immediate AI-Slop Signal)

These fonts are the default output of every AI tool. Using any of them instantly marks the design as generic:

- Inter
- Roboto
- Arial
- Helvetica Neue
- system-ui
- -apple-system
- Open Sans
- Lato
- Montserrat
- Poppins
- Nunito

## Convergence Traps

These fonts are not banned, but AI models tend to default to them across generations. Rotate away — if the last 3 projects used one of these, pick something else:

- Space Grotesk
- DM Sans
- Plus Jakarta Sans
- Outfit
- Manrope
- Sora

## Recommended Fonts

This is not an exhaustive list — discover others. The goal is distinctiveness, not conformity to a different list.

### Display Fonts
Playfair Display, Clash Display, Cabinet Grotesk, General Sans, Satoshi, Instrument Serif, Fraunces, Switzer, Zodiak, Gambetta, Erode, Synonym

### Body Fonts
Source Serif 4, Literata, Figtree, Geist, Nacelle, Wotfard, Supreme, Chillax

### Monospace (When Appropriate)
JetBrains Mono, Berkeley Mono, Commit Mono, Geist Mono

## Font Pairing Strategy

Strong pairings create tension between contrast and harmony:

| Direction | Display | Body | Why It Works |
|-----------|---------|------|-------------|
| Luxury editorial | Instrument Serif | Figtree | Serif elegance + geometric clarity |
| Warm organic | Fraunces | Literata | Both have optical sizing, warm personality |
| Swiss precision | Clash Display | Geist | Sharp geometric + clean neutral |
| Neo-brutalist | Cabinet Grotesk | Source Serif 4 | Grotesk weight + serif readability |
| Retro-futuristic | Zodiak | Satoshi | Distinctive display + clean sans |

## Loading Strategy

```html
<!-- Google Fonts — preconnect for speed -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Display+Font:wght@400;700&family=Body+Font:wght@300;400;500;600&display=swap" rel="stylesheet">
```

```html
<!-- Fontshare — for fonts not on Google Fonts -->
<link href="https://api.fontshare.com/v2/css?f[]=clash-display@400,500,600,700&f[]=satoshi@300,400,500,700&display=swap" rel="stylesheet">
```

## Anti-Patterns

- Using more than 3 fonts on a single page (2 is ideal, 3 max with mono)
- Loading font weights not actually used in the design
- Fallback stacks that are visually different from the intended font (causes layout shift)
- Display fonts below 24px or body fonts above 48px (misusing each font's purpose)
- Identical font-weight for headings and body text (no hierarchy)
