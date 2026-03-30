# Hanuman Chalisa Reader — Design Specification

## Overview
A calm, mobile-first scripture reader for the Hanuman Chalisa. Optimized for phone screens; on desktop, the content is constrained to a mobile-width column (max 448px / `max-w-md`) centered horizontally.

---

## 1. Global Design System

### Font
- **Family:** `Outfit` from Google Fonts (weights 100–900)
- **Import:** Preconnect to `fonts.googleapis.com` and `fonts.gstatic.com`, then load `Outfit:wght@100..900`
- Applied globally via Tailwind `fontFamily.sans: ["Outfit", "sans-serif"]`

### Text Rendering
On `<body>`:
```css
-webkit-font-smoothing: antialiased;
-moz-osx-font-smoothing: grayscale;
text-rendering: optimizeLegibility;
```

### Color Palette (HSL — NO pure black or pure white)
All defined as CSS custom properties in `:root`:

| Token | HSL Value | Usage |
|---|---|---|
| `--background` | `40 20% 96%` | Page background — warm broken white |
| `--foreground` | `30 10% 15%` | Primary text — warm near-black |
| `--primary` | `25 30% 35%` | Brand/accent color — earthy brown |
| `--primary-foreground` | `40 20% 96%` | Text on primary surfaces |
| `--secondary` | `35 15% 90%` | Secondary surfaces |
| `--secondary-foreground` | `30 10% 20%` | Text on secondary surfaces |
| `--muted` | `35 12% 90%` | Muted backgrounds |
| `--muted-foreground` | `30 8% 45%` | Muted/secondary text (used for Hindi subtext, footer ॐ) |
| `--accent` | `30 20% 88%` | Accent surfaces |
| `--accent-foreground` | `30 10% 18%` | Text on accent surfaces |
| `--border` | `35 15% 88%` | Border color |
| `--card` | `40 18% 94%` | Card backgrounds |
| `--ring` | `25 30% 35%` | Focus ring |
| `--radius` | `0.5rem` | Default border radius |

### Key Principle
**No pure `#000` or `#fff` anywhere.** All blacks are warm dark tones (`30 10% 15%`), all whites are warm off-whites (`40 20% 96%`). This creates a calm, paper-like reading experience.

---

## 2. Page Layout

```
┌──────────────────────────────────┐
│         (viewport full width)     │
│  ┌────────────────────────────┐  │
│  │   max-w-md (448px) center  │  │
│  │   px-6 (24px side padding) │  │
│  │   py-12 (48px top/bottom)  │  │
│  │                            │  │
│  │   [Static Header]          │  │
│  │   [Stanzas]                │  │
│  │   [Spacer mt-14]           │  │
│  └────────────────────────────┘  │
│                                  │
│  [STICKY HEADER — fixed top]     │
│  [STICKY FOOTER — fixed bottom]  │
└──────────────────────────────────┘
```

- **Outer container:** `flex justify-center min-h-screen bg-background`
- **Inner content column:** `w-full max-w-md px-6 py-12`

---

## 3. Static Header (in-page, scrolls away)

Centered, at the top of the content column.

### Hindi Title Line
- **Text:** `॥ श्री हनुमान चालीसा ॥` (Devanagari script with double dandas)
- **Styling:** `text-sm font-light tracking-[0.3em] uppercase text-muted-foreground mb-3`
- Size: 14px, weight 300, letter-spacing 0.3em, color muted-foreground (`30 8% 45%`)

### English Title
- **Text:** `Hanuman Chalisa`
- **Element:** `<h1>`
- **Styling:** `text-2xl font-semibold text-foreground tracking-wide`
- Size: 24px, weight 600, color foreground (`30 10% 15%`), slight letter-spacing

### Divider
- **Element:** `<div>` acting as a horizontal rule
- **Styling:** `mt-4 mx-auto w-12 h-px bg-primary/30`
- 48px wide, 1px tall, centered, primary color at 30% opacity
- Margin-top: 16px

### Spacing below header
- `mb-12` (48px) before stanzas begin

---

## 4. Stanzas

### Container
- `space-y-10` — 40px vertical gap between each stanza block

### Individual Stanza
- `text-center space-y-1.5` — centered text, 6px gap between lines within a stanza

### Each Line (`<p>`)
- **Styling:** `text-lg font-normal leading-snug text-foreground/90 tracking-wide`
  - Font size: 18px
  - Font weight: 400 (normal)
  - Line height: `leading-snug` = 1.375
  - Color: foreground at 90% opacity
  - Letter-spacing: slightly wide (`tracking-wide`)

### Stanza Data Structure
Each stanza is an object with a `lines` array of strings. Stanzas are separated by visual spacing only (no dividers, no numbering). Example:
```json
{
  "lines": [
    "Shri Guru Charan Saroj Raj",
    "Nij mane mukure sudhar",
    "Varnao Raghuvar Vimal Jasu",
    "Jo dayaku phal char"
  ]
}
```
Some stanzas have 4 lines, some have 2 — render them all identically.

### Bottom spacer
After all stanzas: `mt-14` (56px) empty div to ensure content doesn't hide behind the sticky footer.

---

## 5. Sticky Header (appears on scroll)

### Behavior
- **Hidden by default** (when `scrollY <= 80px`)
- **Slides down and fades in** when user scrolls past 80px
- Uses a boolean state `scrolled` toggled by a scroll event listener (passive)

### Positioning
- `fixed top-0 left-0 right-0 z-10`

### Background
- `bg-background/90` — the warm off-white at 90% opacity
- `backdrop-blur-sm` — subtle blur for frosted glass effect
- `border-b border-border/50` — bottom border at 50% opacity

### Animation
- `transition-all duration-300`
- When hidden: `opacity-0 -translate-y-full` (slid up and invisible)
- When visible: `opacity-100 translate-y-0` (in place and fully visible)

### Content
- Inner container: `max-w-md mx-auto px-6 py-3 text-center`
- Text: `"Hanuman Chalisa"` (English only, no Hindi)
- Text styling: `text-sm font-medium text-foreground tracking-wide`
  - Size: 14px, weight 500, foreground color

---

## 6. Sticky Footer (appears on scroll)

### Behavior
- Same scroll trigger as header (`scrollY > 80px`)
- **Slides up and fades in** from bottom

### Positioning
- `fixed bottom-0 left-0 right-0 z-10`

### Background
- Same as sticky header: `bg-background/90 backdrop-blur-sm border-t border-border/50`

### Animation
- `transition-all duration-300`
- When hidden: `opacity-0 translate-y-full` (slid down and invisible)
- When visible: `opacity-100 translate-y-0`

### Content Layout
- Inner container: `max-w-md mx-auto px-6 py-3 flex items-center justify-between`
- **Left side:** ॐ symbol
  - Text: `॥ ॐ ॥` (Om flanked by double dandas)
  - Styling: `text-muted-foreground text-xs tracking-[0.2em]`
  - Size: 12px, color muted-foreground, letter-spacing 0.2em
- **Right side:** Scroll-to-top button
  - Icon: `ArrowUp` from `lucide-react`, size 18px, strokeWidth 1.5
  - Color: `text-foreground/90` (matches stanza text color)
  - Hover: `hover:text-foreground` (full opacity)
  - `transition-colors` for smooth hover
  - `aria-label="Scroll to top"`
  - On click: `window.scrollTo({ top: 0, behavior: "smooth" })`

---

## 7. Scroll Logic

```typescript
const [scrolled, setScrolled] = useState(false);

useEffect(() => {
  const onScroll = () => setScrolled(window.scrollY > 80);
  window.addEventListener("scroll", onScroll, { passive: true });
  return () => window.removeEventListener("scroll", onScroll);
}, []);
```

- Threshold: **80px** of scroll before sticky elements appear
- Passive listener for performance
- Cleanup on unmount

---

## 8. Responsive Behavior

- **Mobile (< 448px):** Content fills width with 24px padding on each side
- **Desktop (≥ 448px):** Content is capped at 448px (`max-w-md`) and centered horizontally. The page background fills the full viewport. This simulates a phone-width reading column.

---

## 9. HTML Head Requirements

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@100..900&display=swap" rel="stylesheet">
```

Page title: `Hanuman Chalisa`

---

## 10. Dependencies

- **React** (with hooks: `useState`, `useEffect`)
- **lucide-react** — for `ArrowUp` icon
- **Tailwind CSS** with `tailwindcss-animate` plugin
- **Google Fonts** — Outfit

No other UI libraries required. No shadcn components used in this page.
