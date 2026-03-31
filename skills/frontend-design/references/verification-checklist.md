# Self-Verification Checklist

Run this complete audit before delivering any frontend design. Fix any failures before showing the output.

---

## AI Slop Detection (Must ALL Be FALSE)

If ANY item is TRUE, go back and fix it before delivering.

- [ ] Using a banned font (Inter, Roboto, Arial, system-ui, Poppins, Montserrat, Open Sans, Lato, Nunito)
- [ ] Using a convergence trap font without intentional reason (Space Grotesk, DM Sans, Plus Jakarta Sans, Outfit, Manrope, Sora)
- [ ] Purple/violet gradient anywhere in the design
- [ ] Purple + teal/cyan color combination
- [ ] Generic blue SaaS gradient (#4F46E5 → #06B6D4)
- [ ] Cookie-cutter card grid with identical spacing and structure
- [ ] Three-column feature grid with icon + title + paragraph pattern
- [ ] Meaningless hero animation (floating shapes, particles, orbiting dots without purpose)
- [ ] Scroll-jacking or hijacked scroll behavior
- [ ] Navigation or CTA hidden behind hover state (breaks mobile)
- [ ] 4+ heading styles in a single section
- [ ] Corporate Memphis / flat vector illustration style
- [ ] Identical component spacing throughout (no rhythmic variation)
- [ ] "Powered by AI" aesthetic by default (iridescent, circuit, neural patterns)
- [ ] Full 100vh hero with no visual hint to scroll
- [ ] "Fake dashboard" mockup with generic color-coded callouts
- [ ] Non-system emojis used as decorative elements or pseudo-icons
- [ ] Lazy, predictable icon arrangements
- [ ] Identical sections repeating the same layout structure with different content
- [ ] Colors hardcoded instead of using CSS custom properties
- [ ] More than 3 fonts loaded

---

## Quality Standards (Must ALL Be TRUE)

If ANY item is FALSE, fix it before delivering.

### Design
- [ ] Cohesive aesthetic direction — every element supports the chosen tone
- [ ] Font pairing is distinctive, loads correctly, and not on the banned list
- [ ] Color palette uses CSS custom properties and feels intentional
- [ ] No more than 1-2 accent colors
- [ ] Visual hierarchy guides the eye in a clear path through each section

### Hero
- [ ] Hero H1 clearly communicates what/who/why
- [ ] CTA is visible above the fold
- [ ] Hero is NOT full 100vh (leaves ~10px peek of next section)
- [ ] Subtext is one sentence max

### Motion
- [ ] All animations serve a purpose (guide eye, reinforce hierarchy, feedback, or delight)
- [ ] `prefers-reduced-motion` handled in BOTH CSS and JavaScript
- [ ] Scroll-triggered animations use IntersectionObserver (not scroll event listeners)
- [ ] No animation duration > 600ms on scroll-triggered reveals
- [ ] No scroll-jacking

### Code
- [ ] Semantic HTML: header, nav, main, section, article, footer (not div soup)
- [ ] All colors, fonts, spacing defined as CSS custom properties
- [ ] All buttons/links are clickable and have hover + focus states
- [ ] `:focus-visible` styles on all interactive elements
- [ ] `-webkit-tap-highlight-color: transparent` applied
- [ ] No horizontal scroll on mobile (tested at 320px)
- [ ] Sticky/fixed elements work correctly through full scroll
- [ ] Responsive at 320px, 768px, 1024px, 1440px — nothing breaks

### Conversion
- [ ] Primary CTA uses action verb ("Start free trial", not "Learn more")
- [ ] Trust signals use specific numbers, not vague claims
- [ ] Visual hierarchy: Headline → Value Prop → Social Proof → CTA
- [ ] Form fields minimized (every extra field reduces conversion)

---

## Quick Scan (30-Second Check)

For rapid verification, check these 5 things:

1. **Open DevTools** → search CSS for "Inter", "Roboto", "Arial", "Poppins" → should find ZERO matches
2. **Search CSS for "purple"** or "#7c3aed" or "#8b5cf6" → should find ZERO matches
3. **Resize to 320px width** → no horizontal scroll, all text readable, CTA visible
4. **Scroll the full page** → no jank, no scroll-jacking, all content appears before you reach it
5. **Tab through the page** → every interactive element has a visible focus indicator
