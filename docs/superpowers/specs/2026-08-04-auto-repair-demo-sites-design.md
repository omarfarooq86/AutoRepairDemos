# Auto Repair Demo Websites — Design Spec

**Date:** 2026-08-04
**Purpose:** Two demo landing pages for auto repair shops in Lahore, used in WhatsApp outreach to pitch web design services.

---

## Summary

Two single-page Astro demo sites deployed on Vercel. Each demo targets a different shop type and visual style to maximize pitch flexibility. Real business data is pulled from the Google Maps CSV export (`geoleadscraper-google_maps-20260804142138.csv`). Both sites share components but use separate data configs and CSS so they look completely different.

---

## Architecture

```
/
├── astro.config.mjs
├── package.json
├── vercel.json
├── public/
│   └── images/              ← Hero photos, textures
├── src/
│   ├── components/
│   │   ├── Hero.astro       ← Shop name, tagline, CTA button
│   │   ├── Services.astro   ← Service cards grid (reads from data)
│   │   ├── About.astro      ← Shop description paragraph
│   │   ├── Reviews.astro    ← Rating stars, count, quote snippets
│   │   ├── Contact.astro    ← Address, phone, hours table
│   │   └── Footer.astro     ← Shop name + Veloxa credit
│   ├── layouts/
│   │   └── Base.astro       ← Shared HTML shell, meta tags, favicon
│   ├── pages/
│   │   ├── index.astro      ← Redirect or simple index listing both
│   │   ├── demo-1.astro     ← Al Aziz (industrial style)
│   │   └── demo-2.astro     ← Lahore Best Auto (modern style)
│   ├── data/
│   │   ├── demo-1.json      ← Al Aziz Car Restoration data
│   │   └── demo-2.json      ← Lahore Best Auto Workshop data
│   └── styles/
│       ├── demo-1.css       ← Dark/industrial tokens
│       ├── demo-2.css       ← Clean/modern tokens
│       └── global.css       ← Minimal reset shared by both
```

**Key rule:** Components are shared. Data and CSS make each demo unique. No client-side JS except the embedded Google Map iframe in Contact.

---

## Demo 1 — "The Workshop" (Dark/Industrial)

**Business:** Al Aziz Car Restoration & Workshop
**Route:** `/demo-1`
**Vibe:** Raw, honest, capable — workshop floor meets Swiss typography

### Visual tokens
| Token | Value |
|-------|-------|
| Background | `#1a1a1a` (dark charcoal) |
| Accent | `#f97316` (warm safety orange) |
| Text | `#e5e5e5` (off-white) |
| Muted text | `#888888` |
| Borders | `#333333` |
| Heading font | Bebas Neue or similar condensed sans (CSS: `Impact`/`Arial Black` fallback) |
| Body font | System monospace stack |
| Corner style | Sharp (`border-radius: 0`) |
| Texture | Subtle noise/grain overlay on hero |

### Hero
- Full-width dark background with noise texture
- Small label: "Est. 2018 · Lahore"
- Large shop name in bold condensed type
- Orange accent line below name
- Subtitle: "Car Restoration"
- Orange CTA button: "Call Now — 0321 4619299" with phone icon

### Services grid (2×2)
- Engine Rebuild, Body Work, Restoration, Paint
- Dark cards with orange left-border accent
- Hard grid lines

### About
- Short paragraph from CSV context
- Heavy header, raw text

### Reviews
- Rating: 4.8 ★ (5 reviews)
- One pull-quote in dark card with orange left border
- Google review count link

### Contact
- Address, phone, hours table
- Dark table rows with orange headings
- Embedded Google Maps (light/dark mode iframe to match)

### Footer
- Left: "AL AZIZ CAR RESTORATION"
- Right: "BY VELOXA" in orange

---

## Demo 2 — "The Professional" (Clean/Modern)

**Business:** Lahore Best Auto Workshop
**Route:** `/demo-2`
**Vibe:** Polished, trustworthy, premium — an organized service center

### Visual tokens
| Token | Value |
|-------|-------|
| Background | `#ffffff` (white) |
| Accent | `#1e40af` (deep blue) |
| Text | `#111827` (near-black) |
| Muted text | `#64748b` (slate gray) |
| Card bg | `#f8fafc` (very light gray) |
| Heading font | Inter or system sans-serif |
| Body font | System sans-serif |
| Corner style | Rounded (`border-radius: 8-12px`) |
| Shadows | Subtle card shadows (`box-shadow: 0 1px 3px rgba(0,0,0,0.1)`) |

### Hero
- Light blue gradient background (`#eff6ff` → `#dbeafe`)
- Centered layout
- Rating pill badge: "★ 4.6 · 13 Reviews"
- Shop name in bold Inter-style heading
- Subtitle: "Auto Workshop"
- Blue rounded pill CTA: "Call Now — 0300 4544443"

### Services grid (3×2)
- Brakes, Engine, AC Service, Electrical, Oil Change, Inspection
- Icon + label cards, soft gray backgrounds
- Rounded corners, centered text

### About
- Polished paragraph
- Generous line height and spacing

### Reviews
- Rating: 4.6 ★ (13 reviews)
- Pull-quote in warm yellow card
- Google review count link

### Contact
- Address, phone, hours in clean card layout
- Icons next to each row
- Embedded Google Maps

### Footer
- Left: "LAHORE BEST AUTO WORKSHOP"
- Right: "by Veloxa" in blue

---

## Data Source

Both demo data files are hand-authored from real CSV entries. To personalize for a new lead, edit the JSON file (shop name, phone, address, services) and redeploy — the components read from the config.

### Data file schema (example — `demo-1.json`)

```json
{
  "shop": {
    "name": "Al Aziz Car Restoration & Workshop",
    "shortName": "Al Aziz",
    "tagline": "Car Restoration",
    "established": "2018",
    "municipality": "Canal Bank Housing Scheme"
  },
  "contact": {
    "phone": "03214619299",
    "displayPhone": "0321 4619299",
    "address": "97-A Khuram Block, Taj Chowk, near Lahore Cantt, Lahore 54000",
    "hours": {
      "weekdays": "Open 24 hours",
      "sunday": "Open 24 hours"
    },
    "mapsUrl": "https://www.google.com/maps?cid=6474421547299359523",
    "mapsEmbed": "https://www.google.com/maps/embed?pb=..."  ← derived from place_id in CSV
  },
  "services": ["Engine Rebuild", "Body Work", "Restoration", "Paint"],
  "reviews": {
    "rating": 4.8,
    "count": 5,
    "quote": "Best restoration work in Lahore. Highly recommended."
  }
}
```

---

## Deployment

**Platform:** Vercel
**Framework:** Astro (static output)
**Domains:** `auto-demo-<hash>.vercel.app` (auto Vercel URL), with `/demo-1` and `/demo-2` routes
**Build:** `npm run build` → Vercel auto-detects Astro

---

## Scope

### In scope
- 2 single-page Astro demo sites
- 6 shared components (Hero, Services, About, Reviews, Contact, Footer)
- 2 CSS stylesheets (dark/industrial + clean/modern)
- 2 data JSON files
- Vercel deployment
- Google Maps embed in Contact section
- Mobile-responsive layout

### Out of scope
- Multi-page sub-sites (all content on one scrollable page per demo)
- Contact form (demo only — CTA is phone call)
- CMS or dynamic content
- Analytics
- Custom domain setup

---

## Success Criteria

1. Both demos deploy successfully on Vercel and load with no errors
2. Demo 1 renders in dark/industrial style; Demo 2 renders in clean/modern style
3. All business data (name, phone, services, reviews, address) comes from JSON config files — no hardcoded values in components
4. Sites are mobile-responsive (test at 375px and 1440px)
5. Google Maps embed loads correctly on both pages
6. A new business can be set up by editing one JSON file
