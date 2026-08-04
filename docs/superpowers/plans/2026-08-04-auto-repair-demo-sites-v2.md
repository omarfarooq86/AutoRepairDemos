# Auto Repair Demo Sites v2 — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development.

**Goal:** Rebuild the two demo sites with 11 rich sections each (up from 6), matching UmerAuto.com production quality.

**Architecture:** Enhance 6 existing components, add 5 new ones, expand JSON data files, expand CSS. Same Astro/Vercel stack. All interactive elements are CSS-only (tabs, accordion, mobile menu).

**Tech Stack:** Astro 5, CSS, existing project at `c:\Users\PROBOOK\OneDrive\Desktop\autorepairslahorelead`.

## Global Constraints

- Zero client-side JS except Google Maps iframe — tabs, accordion, mobile nav use CSS-only patterns (`:target`, `details/summary`, `:checked` hack)
- All business data from JSON configs — nothing hardcoded in components
- Mobile-responsive: 375px and 1440px
- Both demos share same 11 components — only CSS and data differ
- No external CSS/font dependencies beyond system stacks and Unsplash images
- Industrial tokens: `#1a1a1a` bg, `#f97316` accent, sharp corners, Impact/Arial Black headings
- Modern tokens: `#ffffff` bg, `#1e40af` accent, rounded + shadows, system sans-serif

---

## File Structure (final state)

```
src/
├── components/
│   ├── Navbar.astro       ← NEW
│   ├── Hero.astro         ← ENHANCE
│   ├── Services.astro     ← ENHANCE
│   ├── WhyChooseUs.astro  ← NEW
│   ├── About.astro        ← ENHANCE (add tabs + stats)
│   ├── WorkProcess.astro  ← NEW
│   ├── CtaBanner.astro    ← NEW
│   ├── Reviews.astro      ← ENHANCE (single → 3 cards)
│   ├── Faq.astro          ← NEW
│   ├── Contact.astro      ← ENHANCE (add display form fields)
│   └── Footer.astro       ← ENHANCE (1-line → 3-column)
├── layouts/
│   └── Base.astro         ← ENHANCE (add viewport meta for mobile nav)
├── pages/
│   ├── index.astro        ← KEEP
│   ├── demo-1.astro       ← UPDATE (imports 11 components)
│   └── demo-2.astro       ← UPDATE
├── data/
│   ├── demo-1.json        ← ENHANCE (expand to full v2 schema)
│   └── demo-2.json        ← ENHANCE
└── styles/
    ├── global.css          ← ENHANCE (add smooth scroll + base nav styles)
    ├── demo-1.css          ← ENHANCE (add all new section styles)
    └── demo-2.css          ← ENHANCE
```

---

### Task 1: Expand data files (demo-1.json, demo-2.json)

**Files:** `src/data/demo-1.json`, `src/data/demo-2.json`

Replace both data files with the full v2 schema including: expanded shop with mission/vision/stats, hero with headline/subheadline/badges, services with descriptions, whyChooseUs array, workProcess array, 3 reviews, faqs array. All values are real content for each business.

- [ ] Write expanded demo-1.json with full v2 schema and real Al Aziz content
- [ ] Write expanded demo-2.json with full v2 schema and real Lahore Best Auto content
- [ ] `npx astro build` — verify JSON parses, build succeeds

---

### Task 2: Enhance global.css and Base.astro

**Files:** `src/styles/global.css`, `src/layouts/Base.astro`

- [ ] Add smooth scroll behavior (`scroll-behavior: smooth`), CSS custom properties for theme colors, base nav reset styles, details/summary base styles for FAQ accordion
- [ ] Verify build

---

### Task 3: Create Navbar component (NEW)

**Files:** Create `src/components/Navbar.astro`

CSS-only sticky navbar with hamburger menu (`input[type=checkbox]:checked ~ .nav-links`). Logo text on left (shop shortName), 6 scroll links on right (Services, About, Process, Reviews, FAQ, Contact). Props: `shop`, `style`.

- [ ] Write Navbar.astro with CSS-only mobile toggle
- [ ] Verify build

---

### Task 4: Enhance Hero component

**Files:** Modify `src/components/Hero.astro`

Add badges row (3 badge pills from hero.badges array) below CTA. Add stats counter row at bottom (4 stat cards from shop.stats array). Keep existing industrial/modern variants. Props: `shop`, `hero`, `contact`, `style`.

- [ ] Enhance Hero.astro with badges + stats
- [ ] Verify build

---

### Task 5: Enhance Services component

**Files:** Modify `src/components/Services.astro`

Each service card now renders: icon, name heading, description paragraph. Props change: `services` now array of `{name, icon, description}` objects.

- [ ] Enhance Services.astro with description per service
- [ ] Verify build

---

### Task 6: Create WhyChooseUs component (NEW)

**Files:** Create `src/components/WhyChooseUs.astro`

3-4 cards in a grid. Each: icon, title, description. Props: `whyChooseUs` array, `style`.

- [ ] Write WhyChooseUs.astro
- [ ] Verify build

---

### Task 7: Enhance About component

**Files:** Modify `src/components/About.astro`

Add 3 CSS-only tabs (About Us / Mission / Vision) using radio buttons + labels. Content changes per tab. Add stats counter row below (4 big numbers from shop.stats). Props: `shop`, `style`.

- [ ] Enhance About.astro with tabs + stats
- [ ] Verify build

---

### Task 8: Create WorkProcess component (NEW)

**Files:** Create `src/components/WorkProcess.astro`

4-step horizontal flow. Each step: number circle, title, description. Connected by a horizontal line or arrow. Props: `workProcess` array, `style`.

- [ ] Write WorkProcess.astro
- [ ] Verify build

---

### Task 9: Create CtaBanner component (NEW)

**Files:** Create `src/components/CtaBanner.astro`

Mid-page CTA strip: heading text + phone button. Simple, shared between demos. Props: `contact` (for phone), `style`.

- [ ] Write CtaBanner.astro
- [ ] Verify build

---

### Task 10: Enhance Reviews component

**Files:** Modify `src/components/Reviews.astro`

Change from 1 review card to 3 cards in a grid. Each: name, date, rating stars, quote. Props: `reviews` array of `{name, rating, text, date}` objects, `style`.

- [ ] Enhance Reviews.astro to render 3 cards
- [ ] Verify build

---

### Task 11: Create Faq component (NEW)

**Files:** Create `src/components/Faq.astro`

5-6 questions using native `<details>/<summary>` elements (CSS-only accordion, zero JS). Props: `faqs` array of `{question, answer}` objects, `style`.

- [ ] Write Faq.astro
- [ ] Verify build

---

### Task 12: Enhance Contact component

**Files:** Modify `src/components/Contact.astro`

Keep existing map + info rows. Add 3 display-only form fields (name, phone, message) — styled but non-functional. Props: `contact`, `style`.

- [ ] Enhance Contact.astro with display form fields
- [ ] Verify build

---

### Task 13: Enhance Footer component

**Files:** Modify `src/components/Footer.astro`

Change from 1-line to 3-column: Quick Links (anchor links), Our Services (list from data), Hours + Phone. Bottom bar: copyright + Veloxa credit. Props: `shop`, `services`, `contact`, `style`.

- [ ] Enhance Footer.astro with 3-column layout
- [ ] Verify build

---

### Task 14: Expand CSS — demo-1 (Industrial)

**Files:** Modify `src/styles/demo-1.css`

Add styles for all new sections: Navbar, WhyChooseUs (cards grid), WorkProcess (step circles + line), CtaBanner, Faq (details/summary), enhanced About (tabs, stats), enhanced Reviews (3-card grid), enhanced Footer (3-column). Also add mobile nav hamburger styles.

- [ ] Add all new industrial section styles
- [ ] Verify build

---

### Task 15: Expand CSS — demo-2 (Modern)

**Files:** Modify `src/styles/demo-2.css`

Same as Task 14 but in modern tokens (white bg, blue accents, rounded, shadows).

- [ ] Add all new modern section styles
- [ ] Verify build

---

### Task 16: Update page routes

**Files:** Modify `src/pages/demo-1.astro`, `src/pages/demo-2.astro`

Both pages import all 11 components (instead of 6) and pass expanded props from the new data schema. demo-1 imports demo-1.css, demo-2 imports demo-2.css.

- [ ] Update demo-1.astro with all 11 components
- [ ] Update demo-2.astro with all 11 components
- [ ] `npx astro build` — verify 3 routes, check dist/ for all sections
- [ ] Visually confirm both demos render correctly

---

### Task 17: Deploy to Vercel

**Files:** None (deploy operation)

- [ ] `npx vercel --prod --yes` to redeploy
- [ ] Verify all routes live
- [ ] Test at 375px and 1440px

---

### Task 18: Update PERSONALIZE.md

**Files:** Modify `PERSONALIZE.md`

Update the guide to reference the expanded JSON schema with all new fields.

- [ ] Update PERSONALIZE.md for v2 schema
- [ ] Commit
