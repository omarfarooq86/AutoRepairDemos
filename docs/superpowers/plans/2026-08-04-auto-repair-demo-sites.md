# Auto Repair Demo Websites — Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build two single-page Astro demo websites for auto repair shops (dark/industrial + clean/modern) and deploy to Vercel.

**Architecture:** Shared Astro components (Hero, Services, About, Reviews, Contact, Footer) render from JSON data configs. Each demo gets its own CSS stylesheet and route. Business data lives in `src/data/demo-N.json` — swap names/numbers by editing one file.

**Tech Stack:** Astro 5, CSS (no framework), Vercel static hosting.

## Global Constraints

- Zero client-side JS except Google Maps embed iframe
- All business data comes from JSON config files — nothing hardcoded in components
- Mobile-responsive: works at 375px and 1440px widths
- Both demos share the same 6 components — only CSS and data differ
- No external CSS/font dependencies beyond system font stacks and Unsplash images
- Vercel auto-detects Astro; no custom build config required

---

## File Structure

```
autorepairslahorelead/
├── astro.config.mjs          ← Astro config (default)
├── package.json              ← Dependencies: astro
├── tsconfig.json             ← Astro default
├── vercel.json               ← Empty object (Vercel auto-detect)
├── public/
│   └── favicon.svg           ← Simple wrench/gear SVG icon
├── src/
│   ├── components/
│   │   ├── Hero.astro        ← Shop name, tagline, CTA button
│   │   ├── Services.astro    ← Service cards grid
│   │   ├── About.astro       ← Shop description paragraph
│   │   ├── Reviews.astro     ← Rating, count, pull-quote
│   │   ├── Contact.astro     ← Address, phone, hours, map iframe
│   │   └── Footer.astro      ← Shop name + Veloxa credit
│   ├── layouts/
│   │   └── Base.astro        ← HTML shell, meta tags, conditional CSS
│   ├── pages/
│   │   ├── index.astro       ← Simple landing listing both demos
│   │   ├── demo-1.astro      ← Al Aziz — loads industrial CSS + data
│   │   └── demo-2.astro      ← Lahore Best Auto — loads modern CSS + data
│   ├── data/
│   │   ├── demo-1.json       ← Al Aziz Car Restoration data
│   │   └── demo-2.json       ← Lahore Best Auto Workshop data
│   └── styles/
│       ├── global.css        ← Minimal reset, box-sizing, body defaults
│       ├── demo-1.css        ← Dark/industrial: #1a1a1a, #f97316, sharp corners
│       └── demo-2.css        ← Clean/modern: #ffffff, #1e40af, rounded, shadows
```

---

### Task 1: Scaffold Astro project

**Files:**
- Create: `package.json`, `astro.config.mjs`, `tsconfig.json`, `vercel.json`
- Create: `src/pages/index.astro` (placeholder)
- Create: `public/favicon.svg`

**Interfaces:**
- Produces: Working `npm run dev` → `http://localhost:4321`

- [ ] **Step 1: Initialize package.json**

In `c:\Users\PROBOOK\OneDrive\Desktop\autorepairslahorelead`, create `package.json`:

```json
{
  "name": "auto-repair-demos",
  "type": "module",
  "version": "0.0.1",
  "scripts": {
    "dev": "astro dev",
    "build": "astro build",
    "preview": "astro preview"
  },
  "dependencies": {
    "astro": "^5.0.0"
  }
}
```

- [ ] **Step 2: Create astro.config.mjs**

```js
import { defineConfig } from 'astro/config';

export default defineConfig({
  output: 'static',
});
```

- [ ] **Step 3: Create tsconfig.json**

```json
{
  "extends": "astro/tsconfigs/base",
  "compilerOptions": {
    "jsx": "preserve"
  }
}
```

- [ ] **Step 4: Create vercel.json**

```json
{}
```

- [ ] **Step 5: Create placeholder index page**

Create `src/pages/index.astro`:

```astro
---
// Placeholder — will be replaced in Task 12
---
<html>
  <body><h1>Auto Repair Demos</h1></body>
</html>
```

- [ ] **Step 6: Create favicon**

Create `public/favicon.svg`:

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24" fill="none" stroke="#f97316" stroke-width="2">
  <circle cx="12" cy="12" r="3"/>
  <path d="M12 1v4M12 19v4M4.22 4.22l2.83 2.83M16.95 16.95l2.83 2.83M1 12h4M19 12h4M4.22 19.78l2.83-2.83M16.95 7.05l2.83-2.83"/>
</svg>
```

- [ ] **Step 7: Install dependencies and verify**

Run: `npm install`
Run: `npx astro build`
Expected: Build succeeds with one page at `dist/index.html`.

---

### Task 2: Create global styles and Base layout

**Files:**
- Create: `src/styles/global.css`
- Create: `src/layouts/Base.astro`

**Interfaces:**
- Produces: `Base.astro` layout accepts `title` (string) and `style` ("industrial" | "modern") props

- [ ] **Step 1: Write global.css**

Create `src/styles/global.css`:

```css
*,
*::before,
*::after {
  box-sizing: border-box;
  margin: 0;
  padding: 0;
}

html {
  scroll-behavior: smooth;
}

body {
  font-family: system-ui, -apple-system, sans-serif;
  line-height: 1.6;
  -webkit-font-smoothing: antialiased;
}

img {
  max-width: 100%;
  display: block;
}

a {
  color: inherit;
  text-decoration: none;
}

section {
  padding: 64px 20px;
  max-width: 1100px;
  margin: 0 auto;
}

@media (max-width: 640px) {
  section {
    padding: 40px 16px;
  }
}
```

- [ ] **Step 2: Write Base.astro**

Create `src/layouts/Base.astro`:

```astro
---
import '../styles/global.css';

export interface Props {
  title: string;
  style: 'industrial' | 'modern';
}

const { title, style } = Astro.props;
---

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>{title} | Auto Workshop</title>
    <link rel="icon" type="image/svg+xml" href="/favicon.svg" />
  </head>
  <body class={`theme-${style}`}>
    <slot />
  </body>
</html>
```

Note: Demo-specific CSS (`demo-1.css` / `demo-2.css`) is imported in each page file (Task 12), not in the layout. Astro bundles CSS only when imported via frontmatter `import`.

- [ ] **Step 3: Verify layout compiles**

Run: `npx astro build`
Expected: Build succeeds (no pages use it yet, but no import errors).

---

### Task 3: Create data files

**Files:**
- Create: `src/data/demo-1.json`
- Create: `src/data/demo-2.json`

**Interfaces:**
- Produces: `shop`, `contact`, `services`, `reviews` objects consumed by all components

- [ ] **Step 1: Write demo-1.json (Al Aziz — Industrial)**

Create `src/data/demo-1.json`:

```json
{
  "style": "industrial",
  "shop": {
    "name": "Al Aziz Car Restoration & Workshop",
    "shortName": "Al Aziz",
    "tagline": "Car Restoration",
    "established": "2018",
    "municipality": "Canal Bank Housing Scheme, Lahore",
    "description": "We specialize in full vehicle restoration, body work, engine rebuilding, and custom paint jobs. With years of hands-on experience and a dedicated team of mechanics, we bring vehicles back to life — from classic rebuilds to modern collision repair. Every job gets our full attention, whether it's a full restoration or a quick touch-up."
  },
  "contact": {
    "phone": "03214619299",
    "displayPhone": "0321 4619299",
    "address": "97-A Khuram Block, Taj Chowk, near Lahore Cantt, Lahore 54000",
    "hours": "Open 24 hours, 7 days a week",
    "mapsEmbed": "https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3402!2d74.4142262!3d31.5731869!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x391911f5bf163fff%3A0x59d9c41824e9cb23!2sAl+Aziz+Car+Restoration+%26+Workshop!5e0!3m2!1sen!2s!4v1712345678901!5m2!1sen!2s"
  },
  "services": [
    "Engine Rebuild",
    "Body Work",
    "Restoration",
    "Paint"
  ],
  "reviews": {
    "rating": 4.8,
    "count": 5,
    "quote": "Best restoration work in Lahore. Highly recommended."
  }
}
```

- [ ] **Step 2: Write demo-2.json (Lahore Best Auto — Modern)**

Create `src/data/demo-2.json`:

```json
{
  "style": "modern",
  "shop": {
    "name": "Lahore Best Auto Workshop",
    "shortName": "Lahore Best Auto",
    "tagline": "Auto Workshop",
    "established": null,
    "municipality": "Cantt, Lahore",
    "description": "Your one-stop auto service center in Lahore. We offer professional brake repair, engine diagnostics, AC servicing, electrical work, oil changes, and vehicle inspections. Our experienced mechanics use modern equipment to get your vehicle running at its best. Fair prices, honest advice, and quality workmanship — that's the Lahore Best Auto promise."
  },
  "contact": {
    "phone": "03004544443",
    "displayPhone": "0300 4544443",
    "address": "13 B, Street 06b, Walton Road, Cantt, Lahore 54000",
    "hours": "Open 24 hours, 7 days a week",
    "mapsEmbed": "https://www.google.com/maps/embed?pb=!1m18!1m12!1m3!1d3403!2d74.3871036!3d31.493144!2m3!1f0!2f0!3f0!3m2!1i1024!2i768!4f13.1!3m3!1m2!1s0x39190f19a3478909%3A0x98ba61f5771a369f!2sLahore+Best+Auto+Workshop!5e0!3m2!1sen!2s!4v1712345678902!5m2!1sen!2s"
  },
  "services": [
    "Brake Repair",
    "Engine Diagnostics",
    "AC Service",
    "Electrical Work",
    "Oil Change",
    "Vehicle Inspection"
  ],
  "reviews": {
    "rating": 4.6,
    "count": 13,
    "quote": "Professional service, fair prices. My go-to workshop in Lahore."
  }
}
```

- [ ] **Step 3: Verify JSON is valid**

Run: `npx astro build`
Expected: Build succeeds (JSON files are valid, no import errors yet).

---

### Task 4: Create Hero component

**Files:**
- Create: `src/components/Hero.astro`

**Interfaces:**
- Consumes: `shop` (name, shortName, tagline, established, municipality), `contact` (displayPhone, phone), `reviews` (rating, count), `style` (string)
- Produces: `<section class="hero">` with shop name, tagline, rating badge, CTA button

- [ ] **Step 1: Write Hero.astro**

Create `src/components/Hero.astro`:

```astro
---
export interface Shop {
  name: string;
  shortName: string;
  tagline: string;
  established: string | null;
  municipality: string;
}

export interface ContactData {
  phone: string;
  displayPhone: string;
}

export interface ReviewData {
  rating: number;
  count: number;
}

export interface Props {
  shop: Shop;
  contact: Pick<ContactData, 'phone' | 'displayPhone'>;
  reviews: Pick<ReviewData, 'rating' | 'count'>;
  style: 'industrial' | 'modern';
}

const { shop, contact, reviews, style } = Astro.props;
---

<section class="hero">
  <div class="hero-inner">
    {style === 'industrial' ? (
      <>
        <p class="hero-est">{shop.established ? `Est. ${shop.established}` : ''}{shop.established && shop.municipality ? ' · ' : ''}{shop.municipality}</p>
        <h1 class="hero-name">{shop.shortName}</h1>
        <div class="hero-line"></div>
        <p class="hero-tagline">{shop.tagline}</p>
      </>
    ) : (
      <>
        <span class="hero-badge">★ {reviews.rating} · {reviews.count} Reviews</span>
        <h1 class="hero-name">{shop.name}</h1>
        <p class="hero-tagline">{shop.tagline}</p>
      </>
    )}
    <a href={`tel:${contact.phone}`} class="hero-cta">Call Now — {contact.displayPhone}</a>
    {style === 'industrial' && (
      <p class="hero-reviews">★ {reviews.rating} ({reviews.count} reviews)</p>
    )}
  </div>
</section>
```

- [ ] **Step 2: Verify component compiles**

Run: `npx astro build`
Expected: Build succeeds (component exists and is syntactically valid — no page imports it yet so it won't appear in output, but Astro checks the file).

---

### Task 5: Create Services component

**Files:**
- Create: `src/components/Services.astro`

**Interfaces:**
- Consumes: `services` (string[]), `style` ("industrial" | "modern")
- Produces: `<section class="services">` with heading and service cards grid

- [ ] **Step 1: Write Services.astro**

Create `src/components/Services.astro`:

```astro
---
export interface Props {
  services: string[];
  style: 'industrial' | 'modern';
}

const { services, style } = Astro.props;
---

<section class="services">
  <h2 class="section-heading">Our Services</h2>
  <div class="services-grid">
    {services.map((svc) => (
      <div class="service-card">
        {style === 'modern' && <span class="service-icon">{getIcon(svc)}</span>}
        <span class="service-label">{svc}</span>
      </div>
    ))}
  </div>
</section>
```

The `getIcon` function maps service names to emoji icons. Add it in the frontmatter script:

```astro
---
// ... (props interface above)

function getIcon(service: string): string {
  const map: Record<string, string> = {
    'brake repair': '\u{1F6E0}\u{FE0F}',
    'engine': '\u{2699}\u{FE0F}',
    'ac service': '\u{2744}\u{FE0F}',
    'electrical': '\u{1F4A1}',
    'oil change': '\u{1F6E2}\u{FE0F}',
    'inspection': '\u{1F50D}',
    'body work': '\u{1F697}',
    'restoration': '\u{1F3A8}',
    'paint': '\u{1F3A8}',
    'engine rebuild': '\u{1F69B}',
    'engine diagnostics': '\u{1F50D}',
  };

  const key = service.toLowerCase();
  for (const [k, v] of Object.entries(map)) {
    if (key.includes(k) || k.includes(key)) return v;
  }
  return '\u{1F527}'; // default: wrench
}
---
```

- [ ] **Step 2: Verify component compiles**

Run: `npx astro build`
Expected: Build succeeds.

---

### Task 6: Create About component

**Files:**
- Create: `src/components/About.astro`

**Interfaces:**
- Consumes: `shop` (shortName, description, tagline), `style` ("industrial" | "modern")
- Produces: `<section class="about">` with heading and description paragraph

- [ ] **Step 1: Write About.astro**

Create `src/components/About.astro`:

```astro
---
export interface Props {
  shop: {
    shortName: string;
    description: string;
    tagline: string;
  };
  style: 'industrial' | 'modern';
}

const { shop, style } = Astro.props;
---

<section class="about">
  <h2 class="section-heading">About {shop.shortName}</h2>
  <div class="about-content">
    <p>{shop.description}</p>
    <p class="about-tagline">We take pride in every vehicle that rolls through our doors. {style === 'industrial' ? 'No shortcuts, no excuses — just honest work.' : 'Quality service, transparent pricing, and a team that cares about your car as much as you do.'}</p>
  </div>
</section>
```

- [ ] **Step 2: Verify component compiles**

Run: `npx astro build`
Expected: Build succeeds.

---

### Task 7: Create Reviews component

**Files:**
- Create: `src/components/Reviews.astro`

**Interfaces:**
- Consumes: `reviews` (rating, count, quote), `style` ("industrial" | "modern")
- Produces: `<section class="reviews">` with rating stars, quote, count

- [ ] **Step 1: Write Reviews.astro**

Create `src/components/Reviews.astro`:

```astro
---
export interface Props {
  reviews: {
    rating: number;
    count: number;
    quote: string;
  };
  style: 'industrial' | 'modern';
}

const { reviews, style } = Astro.props;

function stars(rating: number): string {
  return '\u2605'.repeat(Math.floor(rating)) + (rating % 1 >= 0.5 ? '\u00BD' : '');
}
---

<section class="reviews">
  <h2 class="section-heading">What Our Customers Say</h2>
  <div class="review-card">
    <div class="review-stars">{stars(reviews.rating)}</div>
    <blockquote class="review-quote">&ldquo;{reviews.quote}&rdquo;</blockquote>
    <p class="review-count">{reviews.rating} rating &mdash; based on {reviews.count} Google reviews</p>
  </div>
</section>
```

- [ ] **Step 2: Verify component compiles**

Run: `npx astro build`
Expected: Build succeeds.

---

### Task 8: Create Contact component

**Files:**
- Create: `src/components/Contact.astro`

**Interfaces:**
- Consumes: `contact` (displayPhone, phone, address, hours, mapsEmbed), `style` ("industrial" | "modern")
- Produces: `<section class="contact">` with info rows and Google Maps embed iframe

- [ ] **Step 1: Write Contact.astro**

Create `src/components/Contact.astro`:

```astro
---
export interface ContactData {
  phone: string;
  displayPhone: string;
  address: string;
  hours: string;
  mapsEmbed: string;
}

export interface Props {
  contact: ContactData;
  style: 'industrial' | 'modern';
}

const { contact, style } = Astro.props;
---

<section class="contact">
  <h2 class="section-heading">Visit Us</h2>
  <div class="contact-grid">
    <div class="contact-info">
      <div class="contact-row">
        <strong>Address</strong>
        <span>{contact.address}</span>
      </div>
      <div class="contact-row">
        <strong>Phone</strong>
        <a href={`tel:${contact.phone}`}>{contact.displayPhone}</a>
      </div>
      <div class="contact-row">
        <strong>Hours</strong>
        <span>{contact.hours}</span>
      </div>
    </div>
    <div class="contact-map">
      <iframe
        src={contact.mapsEmbed}
        width="100%"
        height="300"
        style="border:0;"
        allowfullscreen=""
        loading="lazy"
        referrerpolicy="no-referrer-when-downgrade"
        title={`Map showing ${contact.address}`}
      ></iframe>
    </div>
  </div>
</section>
```

- [ ] **Step 2: Verify component compiles**

Run: `npx astro build`
Expected: Build succeeds.

---

### Task 9: Create Footer component

**Files:**
- Create: `src/components/Footer.astro`

**Interfaces:**
- Consumes: `shop` (name), `style` ("industrial" | "modern")
- Produces: `<footer>` with shop name and Veloxa credit

- [ ] **Step 1: Write Footer.astro**

Create `src/components/Footer.astro`:

```astro
---
export interface Props {
  shop: { name: string };
  style: 'industrial' | 'modern';
}

const { shop, style } = Astro.props;
---

<footer class="site-footer">
  <span>{shop.name.toUpperCase()}</span>
  <span class="footer-credit">by <strong>Veloxa</strong></span>
</footer>
```

- [ ] **Step 2: Verify component compiles**

Run: `npx astro build`
Expected: Build succeeds.

---

### Task 10: Create demo-1 CSS (Dark/Industrial)

**Files:**
- Create: `src/styles/demo-1.css`

**Interfaces:**
- Consumed by: `Base.astro` layout when `style === "industrial"`
- Targets: `.theme-industrial` class on `<body>`

- [ ] **Step 1: Write demo-1.css**

Create `src/styles/demo-1.css`:

```css
/* ===== THEME: INDUSTRIAL — Demo 1 ===== */
/* Tokens: bg #1a1a1a | accent #f97316 | text #e5e5e5 | muted #888 | border #333 */

.theme-industrial {
  background: #1a1a1a;
  color: #e5e5e5;
}

/* --- Section Heading --- */
.theme-industrial .section-heading {
  font-family: 'Arial Black', 'Impact', sans-serif;
  font-size: clamp(1.5rem, 4vw, 2rem);
  text-transform: uppercase;
  letter-spacing: 2px;
  color: #f97316;
  margin-bottom: 24px;
  padding-bottom: 12px;
  border-bottom: 2px solid #333;
}

/* --- Hero --- */
.theme-industrial .hero {
  padding: 0;
  max-width: none;
}
.theme-industrial .hero-inner {
  padding: 80px 20px;
  max-width: 1100px;
  margin: 0 auto;
  min-height: 70vh;
  display: flex;
  flex-direction: column;
  justify-content: center;
  background:
    linear-gradient(180deg, rgba(0,0,0,0.4) 0%, rgba(26,26,26,0.95) 100%),
    url('https://images.unsplash.com/photo-1619642751034-765dfdf7c58e?w=1200&q=80') center/cover no-repeat;
  position: relative;
}
.theme-industrial .hero-est {
  font-family: 'Courier New', monospace;
  font-size: 0.75rem;
  color: #f97316;
  letter-spacing: 3px;
  text-transform: uppercase;
  margin-bottom: 12px;
}
.theme-industrial .hero-name {
  font-family: 'Arial Black', 'Impact', sans-serif;
  font-size: clamp(2.5rem, 8vw, 5rem);
  line-height: 1;
  text-transform: uppercase;
  letter-spacing: -2px;
}
.theme-industrial .hero-line {
  width: 80px;
  height: 4px;
  background: #f97316;
  margin: 16px 0;
}
.theme-industrial .hero-tagline {
  font-family: 'Courier New', monospace;
  font-size: 1rem;
  color: #f97316;
  text-transform: uppercase;
  letter-spacing: 3px;
  margin-bottom: 24px;
}
.theme-industrial .hero-cta {
  display: inline-block;
  background: #f97316;
  color: #000;
  font-family: 'Arial Black', 'Impact', sans-serif;
  font-weight: 700;
  font-size: 0.85rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  padding: 14px 28px;
  border: none;
  cursor: pointer;
  transition: background 0.2s;
  width: fit-content;
}
.theme-industrial .hero-cta:hover {
  background: #fb923c;
}
.theme-industrial .hero-reviews {
  margin-top: 12px;
  font-family: 'Courier New', monospace;
  font-size: 0.8rem;
  color: #888;
}

/* --- Services --- */
.theme-industrial .services {
  max-width: 1100px;
  margin: 0 auto;
  padding: 64px 20px;
}
.theme-industrial .services-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1px;
  background: #333;
  border: 1px solid #333;
}
.theme-industrial .service-card {
  background: #1a1a1a;
  padding: 28px 20px;
  text-align: center;
  text-transform: uppercase;
  font-family: 'Courier New', monospace;
  font-size: 0.9rem;
  letter-spacing: 2px;
  color: #e5e5e5;
  transition: color 0.2s, border-color 0.2s;
  border-left: 3px solid transparent;
}
.theme-industrial .service-card:hover {
  color: #f97316;
  border-left-color: #f97316;
}

/* --- About --- */
.theme-industrial .about {
  border-top: 1px solid #333;
}
.theme-industrial .about-content p {
  font-size: 1rem;
  color: #ccc;
  line-height: 1.8;
  max-width: 700px;
}
.theme-industrial .about-tagline {
  margin-top: 16px;
  font-style: italic;
  color: #888 !important;
  font-family: 'Courier New', monospace;
}

/* --- Reviews --- */
.theme-industrial .reviews {
  border-top: 1px solid #333;
}
.theme-industrial .review-card {
  background: #222;
  border-left: 4px solid #f97316;
  padding: 24px;
  max-width: 600px;
}
.theme-industrial .review-stars {
  color: #f97316;
  font-size: 1.2rem;
  margin-bottom: 12px;
}
.theme-industrial .review-quote {
  font-size: 1.1rem;
  color: #ccc;
  font-style: italic;
  line-height: 1.6;
  margin-bottom: 12px;
}
.theme-industrial .review-count {
  font-size: 0.8rem;
  color: #888;
}

/* --- Contact --- */
.theme-industrial .contact {
  border-top: 1px solid #333;
}
.theme-industrial .contact-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 32px;
}
.theme-industrial .contact-row {
  border-bottom: 1px solid #333;
  padding: 16px 0;
}
.theme-industrial .contact-row strong {
  display: block;
  color: #f97316;
  font-family: 'Courier New', monospace;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 2px;
  margin-bottom: 4px;
}
.theme-industrial .contact-row span,
.theme-industrial .contact-row a {
  color: #e5e5e5;
  font-size: 0.95rem;
}
.theme-industrial .contact-map {
  border: 1px solid #333;
}
.theme-industrial .contact-map iframe {
  filter: grayscale(100%) invert(0.9);
}

/* --- Footer --- */
.theme-industrial .site-footer {
  max-width: 1100px;
  margin: 0 auto;
  padding: 24px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid #333;
  font-family: 'Courier New', monospace;
  font-size: 0.7rem;
  color: #666;
  text-transform: uppercase;
  letter-spacing: 2px;
}
.theme-industrial .footer-credit strong {
  color: #f97316;
}

/* --- Mobile --- */
@media (max-width: 640px) {
  .theme-industrial .hero-inner {
    padding: 60px 16px;
    min-height: 60vh;
  }
  .theme-industrial .services-grid {
    grid-template-columns: 1fr;
  }
  .theme-industrial .contact-grid {
    grid-template-columns: 1fr;
  }
  .theme-industrial .site-footer {
    flex-direction: column;
    gap: 8px;
    text-align: center;
  }
}
```

- [ ] **Step 2: Verify CSS is valid (no build errors)**

Run: `npx astro build`
Expected: Build succeeds with no CSS parsing errors.

---

### Task 11: Create demo-2 CSS (Clean/Modern)

**Files:**
- Create: `src/styles/demo-2.css`

**Interfaces:**
- Consumed by: `Base.astro` layout when `style === "modern"`
- Targets: `.theme-modern` class on `<body>`

- [ ] **Step 1: Write demo-2.css**

Create `src/styles/demo-2.css`:

```css
/* ===== THEME: MODERN — Demo 2 ===== */
/* Tokens: bg #fff | accent #1e40af | text #111827 | muted #64748b | card #f8fafc */

.theme-modern {
  background: #ffffff;
  color: #111827;
}

/* --- Section Heading --- */
.theme-modern .section-heading {
  font-size: clamp(1.5rem, 4vw, 2rem);
  font-weight: 700;
  color: #111827;
  margin-bottom: 24px;
  letter-spacing: -0.5px;
}

/* --- Hero --- */
.theme-modern .hero {
  padding: 0;
  max-width: none;
}
.theme-modern .hero-inner {
  padding: 80px 20px;
  max-width: 1100px;
  margin: 0 auto;
  min-height: 60vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  text-align: center;
  background:
    linear-gradient(135deg, #eff6ff 0%, #dbeafe 100%);
  border-radius: 0 0 24px 24px;
}
.theme-modern .hero-badge {
  display: inline-block;
  background: #fff;
  border: 1px solid #dbeafe;
  border-radius: 100px;
  padding: 6px 16px;
  font-size: 0.8rem;
  font-weight: 500;
  color: #1e40af;
  margin-bottom: 16px;
}
.theme-modern .hero-name {
  font-size: clamp(2rem, 6vw, 3.5rem);
  font-weight: 800;
  line-height: 1.1;
  letter-spacing: -1px;
  margin-bottom: 4px;
}
.theme-modern .hero-tagline {
  font-size: 1.1rem;
  color: #64748b;
  margin-bottom: 24px;
}
.theme-modern .hero-cta {
  display: inline-block;
  background: #1e40af;
  color: #fff;
  font-size: 0.95rem;
  font-weight: 600;
  padding: 14px 32px;
  border-radius: 100px;
  transition: background 0.2s;
  box-shadow: 0 2px 8px rgba(30, 64, 175, 0.25);
}
.theme-modern .hero-cta:hover {
  background: #1e3a8a;
}

/* --- Services --- */
.theme-modern .services {
  max-width: 1100px;
  margin: 0 auto;
  padding: 64px 20px;
}
.theme-modern .services-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 12px;
}
.theme-modern .service-card {
  background: #f8fafc;
  border: 1px solid #f1f5f9;
  border-radius: 12px;
  padding: 24px 16px;
  text-align: center;
  transition: box-shadow 0.2s, border-color 0.2s;
}
.theme-modern .service-card:hover {
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.08);
  border-color: #dbeafe;
}
.theme-modern .service-icon {
  display: block;
  font-size: 1.5rem;
  margin-bottom: 8px;
}
.theme-modern .service-label {
  font-size: 0.85rem;
  font-weight: 500;
  color: #334155;
}

/* --- About --- */
.theme-modern .about {
  background: #f8fafc;
  max-width: none;
  padding: 64px 20px;
}
.theme-modern .about .section-heading,
.theme-modern .about-content {
  max-width: 1100px;
  margin-left: auto;
  margin-right: auto;
}
.theme-modern .about-content p {
  font-size: 1.05rem;
  color: #475569;
  line-height: 1.8;
  max-width: 700px;
}
.theme-modern .about-tagline {
  margin-top: 16px;
  font-style: italic;
  color: #1e40af !important;
}

/* --- Reviews --- */
.theme-modern .reviews {
  max-width: 1100px;
  margin: 0 auto;
}
.theme-modern .review-card {
  background: #fefce8;
  border: 1px solid #fde68a;
  border-radius: 12px;
  padding: 24px;
  max-width: 600px;
}
.theme-modern .review-stars {
  color: #eab308;
  font-size: 1.2rem;
  margin-bottom: 12px;
}
.theme-modern .review-quote {
  font-size: 1.1rem;
  color: #713f12;
  font-style: italic;
  line-height: 1.6;
  margin-bottom: 12px;
}
.theme-modern .review-count {
  font-size: 0.85rem;
  color: #1e40af;
  font-weight: 500;
}

/* --- Contact --- */
.theme-modern .contact {
  max-width: 1100px;
  margin: 0 auto;
}
.theme-modern .contact-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 32px;
}
.theme-modern .contact-row {
  padding: 16px 0;
  border-bottom: 1px solid #f1f5f9;
}
.theme-modern .contact-row strong {
  display: block;
  color: #64748b;
  font-size: 0.75rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  margin-bottom: 4px;
  font-weight: 600;
}
.theme-modern .contact-row span,
.theme-modern .contact-row a {
  color: #111827;
  font-size: 1rem;
}
.theme-modern .contact-row a {
  color: #1e40af;
  font-weight: 500;
}
.theme-modern .contact-map {
  border-radius: 12px;
  overflow: hidden;
  box-shadow: 0 1px 3px rgba(0, 0, 0, 0.1);
}

/* --- Footer --- */
.theme-modern .site-footer {
  max-width: 1100px;
  margin: 0 auto;
  padding: 24px 20px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  border-top: 1px solid #f1f5f9;
  font-size: 0.75rem;
  color: #94a3b8;
}
.theme-modern .footer-credit strong {
  color: #1e40af;
}

/* --- Mobile --- */
@media (max-width: 640px) {
  .theme-modern .hero-inner {
    padding: 60px 16px;
    min-height: 50vh;
  }
  .theme-modern .services-grid {
    grid-template-columns: repeat(2, 1fr);
  }
  .theme-modern .contact-grid {
    grid-template-columns: 1fr;
  }
  .theme-modern .site-footer {
    flex-direction: column;
    gap: 8px;
    text-align: center;
  }
}
```

- [ ] **Step 2: Verify CSS is valid**

Run: `npx astro build`
Expected: Build succeeds.

---

### Task 12: Create page routes

**Files:**
- Modify: `src/pages/index.astro` (replace placeholder)
- Create: `src/pages/demo-1.astro`
- Create: `src/pages/demo-2.astro`

**Interfaces:**
- Consumes: All components + data files
- Produces: Three routes: `/`, `/demo-1`, `/demo-2`

- [ ] **Step 1: Write index.astro (landing page)**

Replace the placeholder in `src/pages/index.astro`:

```astro
---
import Base from '../layouts/Base.astro';
---

<Base title="Auto Repair Demos" style="modern">
  <main style="display:flex;flex-direction:column;align-items:center;justify-content:center;min-height:80vh;text-align:center;padding:40px 20px">
    <h1 style="font-size:2rem;font-weight:700;margin-bottom:8px">Auto Repair Demo Sites</h1>
    <p style="color:#64748b;margin-bottom:32px">Two demo websites for WhatsApp outreach</p>
    <div style="display:flex;gap:16px;flex-wrap:wrap;justify-content:center">
      <a href="/demo-1" style="display:inline-block;background:#1a1a1a;color:#f97316;padding:14px 28px;font-weight:700;text-transform:uppercase;letter-spacing:1px;font-size:0.85rem">Demo 1 — Industrial</a>
      <a href="/demo-2" style="display:inline-block;background:#1e40af;color:#fff;padding:14px 28px;font-weight:600;border-radius:100px;font-size:0.9rem">Demo 2 — Modern</a>
    </div>
  </main>
</Base>
```

- [ ] **Step 2: Write demo-1.astro**

Create `src/pages/demo-1.astro`:

```astro
---
import '../styles/demo-1.css';
import Base from '../layouts/Base.astro';
import Hero from '../components/Hero.astro';
import Services from '../components/Services.astro';
import About from '../components/About.astro';
import Reviews from '../components/Reviews.astro';
import Contact from '../components/Contact.astro';
import Footer from '../components/Footer.astro';
import data from '../data/demo-1.json';

const { shop, contact, services, reviews, style } = data;
---

<Base title={shop.name} style={style}>
  <Hero shop={shop} contact={contact} reviews={reviews} style={style} />
  <Services services={services} style={style} />
  <About shop={shop} style={style} />
  <Reviews reviews={reviews} style={style} />
  <Contact contact={contact} style={style} />
  <Footer shop={shop} style={style} />
</Base>
```

- [ ] **Step 3: Write demo-2.astro**

Create `src/pages/demo-2.astro`:

```astro
---
import '../styles/demo-2.css';
import Base from '../layouts/Base.astro';
import Hero from '../components/Hero.astro';
import Services from '../components/Services.astro';
import About from '../components/About.astro';
import Reviews from '../components/Reviews.astro';
import Contact from '../components/Contact.astro';
import Footer from '../components/Footer.astro';
import data from '../data/demo-2.json';

const { shop, contact, services, reviews, style } = data;
---

<Base title={shop.name} style={style}>
  <Hero shop={shop} contact={contact} reviews={reviews} style={style} />
  <Services services={services} style={style} />
  <About shop={shop} style={style} />
  <Reviews reviews={reviews} style={style} />
  <Contact contact={contact} style={style} />
  <Footer shop={shop} style={style} />
</Base>
```

- [ ] **Step 4: Build and verify all routes**

Run: `npx astro build`
Expected: Build succeeds. Output in `dist/` should contain:
- `dist/index.html`
- `dist/demo-1/index.html`
- `dist/demo-2/index.html`

---

### Task 13: Deploy to Vercel

**Files:**
- Create: `.gitignore`
- Create: `README.md`

**Interfaces:**
- Produces: Live Vercel URL with both demos accessible

- [ ] **Step 1: Create .gitignore**

Create `.gitignore`:

```
node_modules/
dist/
.superpowers/
```

- [ ] **Step 2: Initialize git and commit**

```bash
git init
git add -A
git commit -m "feat: two auto repair shop demo sites — industrial + modern"
```

- [ ] **Step 3: Install Vercel CLI and deploy**

```bash
npm i -g vercel
vercel --prod
```

Follow the Vercel CLI prompts:
- Set up and deploy: `Y`
- Which scope: (select your account)
- Link to existing project: `N`
- Project name: `auto-repair-demos` (or press Enter for auto)
- In which directory: `.` (current)
- Override settings: `N`

Expected: Vercel outputs a production URL like `https://auto-repair-demos-XXXX.vercel.app`.

- [ ] **Step 4: Verify live deployment**

Open the production URL and check:
- `/` — landing page loads with links to both demos
- `/demo-1` — Al Aziz industrial site renders correctly
- `/demo-2` — Lahore Best Auto modern site renders correctly
- Both sites are mobile-responsive (test in Chrome DevTools at 375px width)
- Google Maps embeds load on both pages
- Phone CTA buttons open `tel:` links correctly

---

### Task 14: Create personalization guide

**Files:**
- Create: `PERSONALIZE.md`

**Interfaces:**
- None — documentation only

- [ ] **Step 1: Write PERSONALIZE.md**

Create `PERSONALIZE.md`:

```markdown
# How to Personalize a Demo for a New Lead

1. Open the matching data file (`src/data/demo-1.json` for industrial, `src/data/demo-2.json` for modern)
2. Replace the fields with the new shop's info:
   - `shop.name`, `shop.shortName`, `shop.tagline` — shop name and subtitle
   - `contact.phone`, `contact.displayPhone` — phone number
   - `contact.address`, `contact.hours` — location and timings
   - `contact.mapsEmbed` — Google Maps embed URL for their location
   - `services` — list of 4-6 services they offer
   - `reviews.rating`, `reviews.count`, `reviews.quote` — rating + a pull-quote
3. Save the file
4. Run `npx astro build` to verify it compiles
5. Deploy: `vercel --prod`
6. Send the prospect the URL: `https://your-project.vercel.app/demo-1`
```

- [ ] **Step 2: Commit guide**

```bash
git add PERSONALIZE.md
git commit -m "docs: add personalization guide"
```
