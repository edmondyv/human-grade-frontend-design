# Example: Dashboard / Web App

## User Request
"Build a dashboard for tracking content performance — shows prompt usage, AI detection scores, and humanization stats."

## Design Decisions

### 1. Purpose & User
- **What**: Internal analytics dashboard for content teams
- **Who**: Marketing managers, content leads — data-savvy but not developers
- **Emotional state**: Task-focused, wants quick answers, not impressed by flashiness
- **Action**: Scan metrics, drill into details, export reports

### 2. Aesthetic Direction
**Swiss precision** — mathematical grid, monospace data, generous whitespace. Feels like an instrument panel, not a toy. Dark theme for long-session comfort.

### 3. Memory Test
Oversized single-metric hero card showing today's key number with a subtle trend sparkline.

### 4. Anti-Default Check
- Font: NOT system-ui → Geist (body) + Geist Mono (data) + Clash Display (headings)
- Color: NOT generic blue dashboard → near-black + single lime accent for positive metrics
- Layout: NOT cramped 4-column cards → generous sidebar + focused main panel
- Charts: NOT rainbow-colored → monochrome with ONE accent color for highlights

## Structure

```
[Sidebar: 240px fixed]
  Logo
  Navigation: Overview / Prompts / Detection / Humanizer / Settings
  User avatar + name at bottom

[Main content area]
  [Header bar]
    Page title / Date range picker / Export button

  [Hero metric card: full width]
    "2,847 prompts enhanced today" + sparkline + % change

  [Metric row: 3 cards]
    Avg quality score / Detection accuracy / Humanization rate
    Each: number + mini chart + trend indicator

  [Main chart area: responsive height]
    Line/area chart with single accent color
    Hover tooltips with precise values

  [Recent activity table]
    Timestamp / Action / Input preview / Score / Status
    Sortable columns, hover rows, pagination

  [Footer: minimal — version + help link]
```

## Key Technical Choices

- **CSS Grid** for overall layout (sidebar + main)
- **CSS custom properties** including dark theme tokens
- **Monospace for all data values** (visual consistency in numbers)
- **Minimal animation**: fade-in on page load, counter animation on metrics, hover states
- **No decorative elements**: every pixel serves the data
- **Keyboard navigation**: full tab support, arrow keys for table rows
- **Responsive**: sidebar collapses to top bar on tablet, stacks on mobile

## Dashboard-Specific Rules

- Data density > visual decoration. Never sacrifice information for aesthetics.
- Charts use ONE color with opacity variations, not rainbow palettes.
- Tables are scannable: consistent column widths, alternating row backgrounds (subtle).
- Loading states: skeleton screens, not spinners.
- Empty states: helpful message with action, not just "No data".
- Errors: inline, specific, and actionable — not generic toasts.
