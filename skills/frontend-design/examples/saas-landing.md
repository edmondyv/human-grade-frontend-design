# Example: SaaS Landing Page

## User Request
"Build a landing page for a prompt enhancement tool called GPT Upgrader. It has a prompt enhancer, AI detector, AI humanizer, and bundle pricing."

## Design Decisions

### 1. Purpose & User
- **What**: AI prompt toolkit — enhances prompts, detects AI text, humanizes AI output
- **Who**: Content creators, marketers, students — people who use AI daily and want better results
- **Emotional state**: Frustrated with weak AI output, curious about improvement
- **Action**: Start free trial

### 2. Aesthetic Direction
**Warm organic** — earth tones, cream backgrounds, terracotta accents. Feels trustworthy and human (ironic counterpoint to an AI tool). Avoids the typical dark/neon "tech tool" look.

### 3. Memory Test
A before/after prompt comparison with typewriter animation showing the transformation in real-time.

### 4. Anti-Default Check
- Font: NOT Inter/Roboto → Fraunces (display) + Literata (body)
- Color: NOT purple gradient → cream + terracotta + charcoal
- Layout: NOT 3-column icon grid → alternating feature blocks with asymmetric positioning
- Hero: NOT vague "AI-Powered Solutions" → specific "Write smarter. Sound human. Every time."

## Structure

```
[Nav: Logo — Features / Pricing / FAQ — CTA button]

[Hero: calc(100vh - 10px)]
  H1: "Write smarter. Sound human. Every time."
  Sub: "Enhance any prompt, detect AI text, and humanize output — all in one toolkit."
  CTA: "Start Free Trial" (primary) + "See it in action" (secondary)
  Visual: Before/after prompt transformation demo

[Social proof bar]
  "94% prompt quality improvement" / "12,000+ users" / "3-second processing"

[Features: alternating left/right blocks]
  Prompt Enhancer — with inline demo
  AI Detector — with confidence meter visual
  AI Humanizer — with before/after text
  Bundle Builder — with drag-and-drop illustration

[Pricing: 3 tiers]
  Starter $9/mo / Pro Bundle $24/mo (featured) / Team $18/seat/mo
  Pro tier: elevated card, accent border, "Most Popular" badge

[Testimonials: staggered grid, 2-3 quotes with real names]

[Final CTA: "Ready to upgrade every prompt?"]

[Footer: Links / Legal / Newsletter]
```

## Key Technical Choices

- **Single-file HTML** with all CSS/JS inline
- **CSS custom properties** for entire palette
- **IntersectionObserver** for scroll reveals (0.15 threshold, 400ms duration)
- **`prefers-reduced-motion`** kills all animations, shows static content
- **Noise texture overlay** via inline SVG
- **Semantic HTML** throughout
- **Responsive**: 320px → 768px → 1024px → 1440px
