# Hanuman Chalisa Reader

A calm, mobile-first scripture reader for the Hanuman Chalisa, designed for focused reading on phone screens while staying centered and comfortable on desktop.

## Features
- Mobile-first layout with a centered reading column (`max-width: 448px`)
- Warm paper-like color system using HSL design tokens
- Static in-page title section (Hindi + English)
- Verse-by-verse stanza rendering with consistent spacing
- Sticky header/footer that appear after scrolling
- Smooth scroll-to-top action in footer
- Clean typography using the Outfit font

## Project Structure
- `index.html` - page markup, stanza data, and scroll behavior
- `index.css` - design system tokens and all visual styling
- `hanuman-chalisa-design-guide.md` - source design specification
- `Sri_Hanuman_Chalisa_non_english.md` - extracted transliterated verses

## Run Locally
No build step is required.

1. Open `index.html` directly in your browser, or
2. Serve the folder with any static server.

Example with Python:

```bash
python3 -m http.server 8080
```

Then visit `http://localhost:8080`.

## Design Notes
- Avoids pure black and pure white for a softer reading experience
- Keeps content narrow on desktop to preserve a phone-like reading rhythm
- Uses subtle transitions and blur on sticky chrome for minimal distraction

## Git Notes
Ignored files/directories:
- `Sri_Hanuman_Chalisa_English.pdf`
- `.venv/`
