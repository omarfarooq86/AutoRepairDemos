# Auto Repair Demo Websites v2 — Design Spec

**Date:** 2026-08-04
**Purpose:** Rebuild the two demo landing pages with 11 rich sections (up from 6), matching the depth and production quality of UmerAuto.com.

---

## What Changes from v1

| v1 | v2 |
|---|---|
| 6 bare sections | 11 detailed sections |
| Services = label only | Services = icon + paragraph description |
| 1 review quote | 3 review cards with name/date |
| About = 1 paragraph | About = tabs + stats counter row |
| No navbar | Sticky navbar with smooth scroll |
| No FAQ / Work Process / Why Choose Us / CTA | All added |
| Footer = 1 line | Footer = 3-column (links + services + hours) |

---

## Architecture

Same approach as v1: shared Astro components, separate JSON data configs, separate CSS per demo. Routes: `/demo-1` (Al Aziz, industrial) and `/demo-2` (Lahore Best Auto, modern).

```
src/components/
├── Navbar.astro       ← NEW
├── Hero.astro         ← ENHANCED
├── Services.astro     ← ENHANCED
├── WhyChooseUs.astro  ← NEW
├── About.astro        ← ENHANCED
├── WorkProcess.astro  ← NEW
├── CtaBanner.astro    ← NEW
├── Reviews.astro      ← ENHANCED
├── Faq.astro          ← NEW
├── Contact.astro      ← ENHANCED
└── Footer.astro       ← ENHANCED
```

11 components total (5 new, 6 enhanced). Each demo page imports all 11 + its CSS + its data.

---

## Section Designs

Both demos share the same component structure. Styles differ per theme.

### 1. Navbar (NEW)
- Sticky top bar, dark bg (industrial) or white bg with shadow (modern)
- Left: shop short name as logo text
- Right: smooth-scroll links (Services, About, Process, Reviews, FAQ, Contact)
- Mobile: hamburger to dropdown (CSS-only, no JS beyond anchor behavior)

### 2. Hero (ENHANCED)
- Adds hero badges row below CTA (e.g., "24/7 Service · Genuine Parts · Free Inspection")
- Adds stats counter row at bottom of hero (Years, Cars Serviced, Team, Rating)
- Industrial: full-bleed dark photo, orange accent badges
- Modern: blue gradient, white pill badges

### 3. Services (ENHANCED)
- Each service card now has: icon, name, 1-2 sentence description
- Industrial: 2×2 grid, dark cards, orange left border on hover
- Modern: 3×2 grid, light cards with blue icon circles, shadow on hover

### 4. Why Choose Us (NEW)
- 3-4 cards highlighting differentiators
- Industrial: dark cards with orange icon accent, numbered
- Modern: light cards, blue icon circles, subtle shadow

### 5. About (ENHANCED)
- Tabs: About Us / Our Mission / Our Vision (CSS-only tab switching)
- Content changes per tab click
- Stats counter row below: 4 big numbers with labels
- Industrial: dark section, orange tab active state
- Modern: light gray section bg, blue tab active state

### 6. Work Process (NEW)
- 4-step horizontal flow: Diagnose → Plan → Execute → Deliver
- Each step: number circle + title + short description
- Industrial: orange number circles, dark cards, connecting line
- Modern: blue number circles, white cards, subtle connecting line

### 7. CTA Banner (NEW)
- Mid-page call-to-action strip
- "Ready to get your car serviced?" + phone CTA button
- Industrial: dark bg, orange CTA
- Modern: blue gradient bg, white CTA

### 8. Reviews (ENHANCED)
- 3 review cards instead of 1
- Each card: name, date, rating stars, quote text
- Industrial: dark cards, orange stars
- Modern: white cards with border, blue-tinted stars

### 9. FAQ (NEW)
- 5-6 questions, CSS-only accordion (details/summary elements)
- Auto repair relevant Q&A
- Industrial: dark section, orange expand icons
- Modern: light section, blue expand icons, card-style items

### 10. Contact (ENHANCED)
- Existing: map iframe + address/phone/hours
- Adds: display-only form fields (name, phone, message — visual only, no submit) to look like a real contact form
- Industrial: dark form fields, orange labels
- Modern: light form fields, blue labels

### 11. Footer (ENHANCED)
- 3-column layout: Quick Links (anchor links), Our Services (list), Hours + phone
- Bottom bar: copyright + Veloxa credit
- Industrial: dark bg, orange accents
- Modern: dark blue bg, white text

---

## Data Schema

Each demo's JSON expands to include all new sections. Key additions:

```json
{
  "style": "industrial",
  "shop": {
    "name", "shortName", "tagline", "established", "municipality",
    "description", "mission", "vision",
    "stats": [
      { "label": "Years Experience", "value": "8+" },
      { "label": "Cars Serviced", "value": "5,000+" },
      { "label": "Team Members", "value": "12" },
      { "label": "Google Rating", "value": "4.8★" }
    ]
  },
  "hero": {
    "headline": "Lahore's Trusted Car Restoration Experts",
    "subheadline": "...",
    "badges": ["24/7 Service", "Genuine Parts", "Free Inspection"]
  },
  "services": [
    { "name": "...", "icon": "engine", "description": "..." }
  ],
  "whyChooseUs": [
    { "title": "...", "icon": "badge", "description": "..." }
  ],
  "workProcess": [
    { "step": 1, "title": "Diagnose", "description": "..." }
  ],
  "reviews": [
    { "name": "...", "rating": 5, "text": "...", "date": "..." }
  ],
  "faqs": [
    { "question": "...", "answer": "..." }
  ],
  "contact": {
    "phone", "displayPhone", "address", "hours", "mapsEmbed"
  }
}
```

---

## Visual Tokens (unchanged from v1)

**Demo 1 (Industrial):** bg `#1a1a1a` | accent `#f97316` | text `#e5e5e5` | border `#333` | sharp corners | Impact/Arial Black headings | Courier New details

**Demo 2 (Modern):** bg `#ffffff` | accent `#1e40af` | text `#111827` | muted `#64748b` | rounded + shadows | system sans-serif

---

## Scope

**In scope:** 11 components, 2 enhanced JSON data files, 2 updated CSS files, 2 page routes, Vercel redeploy

**Out of scope:** Multi-page sub-sites, functional contact form, blog, portfolio, real team photos, payment/pricing

---

## Success Criteria

1. Each demo has 11 distinct, content-rich sections
2. All business data comes from JSON — no hardcoded values in components
3. Mobile-responsive at 375px and 1440px
4. Zero client-side JS except Google Maps iframe and CSS-only interactive elements
5. Both demos deploy and render correctly on Vercel
6. Personalizing for a new lead = edit one JSON file
